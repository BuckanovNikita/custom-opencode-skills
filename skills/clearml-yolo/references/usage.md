# Command and configuration recipes

These examples assume the installed commands are available. Replace paths and run
identity with actual inputs. Commands without `--help` or `--cfg` execute real work.
Do not install optional backends or change dependency revisions as a usage side effect.

## Select a stage

| Command | Required input and result |
| --- | --- |
| `cy` | Annotation CSV; trains, predicts, calibrates/evaluates, optionally compares and reports |
| `cy-train` | Annotation CSV; trained checkpoint and prepared dataset |
| `cy-predict` | CSV identifying images and checkpoint; prediction CSV |
| `cy-val` | Labeled CSV and checkpoint; predictions and frozen-threshold evaluation |
| `cy-metrics` | Ground-truth and prediction CSVs; thresholds, evaluation payloads/workbooks |
| `cy-compare` | Current labeled CSV and model references; paired comparison and manifest |
| `cy-report` | `comparison_dir` containing paired evaluated results/manifest; report workbooks |
| `cy-ground-truth` | YOLO `data_yaml`; annotation CSV conversion |
| `cy-export` | Faster R-CNN checkpoint or export intermediate, depending on operation |
| `cy-init-config` | Destination and selected command; editable YAML, no tracking task |
| `cy-dedup` | Optional cache directory; reflink maintenance, `--dry-run` previews |
| `cy-queue` | Submission/worker/state operations; separate durable scheduling |

Execution stages require ClearML. Configure credentials through the installation's
normal SDK configuration; never put secrets in examples or logs. `clearml.output_uri`
defaults to true; null, false and empty destinations are rejected. FiftyOne defaults
enabled on commands exposing that configuration group; `cy-val` has no such group.

## Full YOLO pipeline

```bash
cy-init-config cy-config
```

Edit `cy-config/cy.yaml`, preserving the generated defaults and other settings:

```yaml
ground_truth: ./ground_truth.csv
clearml:
  project_name: detection
  task_name: first-experiment
  tags: [experiment]
fiftyone:
  enabled: false
```

Edit bare native keys in `cy-config/ultralytics/default.yaml`:

```yaml
model: yolo11n.pt
epochs: 10
imgsz: 640
batch: 8
device: cpu
```

Edit `cy-config/ultralytics_predict/default.yaml`:

```yaml
batch: 8
conf: 0.001
iou: 0.7
device: cpu
```

CPU is an explicit example device, not a dry run. Select actual devices from the
user's request. Training/inference device settings are independent. Native `data`,
`source` and pipeline output routing are wrapper-owned. Known native Hydra overrides
normally need no `+` prefix; keep upstream keys at each group file's root.

Compose before an authorized run, then execute:

```bash
cy --config-dir cy-config --config-name cy --cfg job
cy --config-dir cy-config --config-name cy run_dir=./runs/experiment-01
```

The pipeline calibrates on `val`. Automatic comparison finds the latest completed
`prod` task in the same project, excluding the current task, and reinfers that model
on current `test` images. No automatic baseline means comparison/reports are skipped.
An explicit unavailable baseline is an error:

```bash
cy --config-dir cy-config --config-name cy compare.baseline_model.task_id=TASK_ID
```

## CSV and path contracts

Training uses this header and absolute pixel corner coordinates:

```csv
image_name,image_path,instance_label,bbox_x_tl,bbox_y_tl,bbox_x_br,bbox_y_br,split
train.jpg,images/train.jpg,helmet,10,20,100,120,train
val.jpg,images/val.jpg,helmet,10,20,100,120,val
test.jpg,images/test.jpg,helmet,10,20,100,120,test
```

These rows describe existing images; they do not create fixture data. CSV image paths
resolve relative to the CSV directory; the CSV argument resolves from command cwd.
`image_name` must match the resolved basename. One image identity cannot refer to
different paths or splits. Background records have blank label and coordinates;
do not mix background and annotated rows for the same image.
Literal labels `NA`, `NULL`, `nan`, `None`, numeric-looking strings such as `0001`
and Unicode remain strings; do not replace them with missing values. Detection
readers preserve row order, empty backgrounds and round-trip numeric precision
through cache reloads and result publication. Standalone prediction/comparison
and `skip_train` resolve media from the CSV directory even after a Hydra cwd change;
source exports retain their original lexical paths and row lineage.
Standalone training requires nonempty train/val; default pipeline needs train/val/test.
Invalid boxes are dropped with a count; no valid training boxes is fatal. CSV mode
rejects resume. Do not modify source images to repair cached working copies.

For a cveta2 dataset export, inspect its actual CSV header, labels, image paths and
split assignments against this contract before handing it to training. An export
alone does not establish valid train/val/test populations or accessible images.
Preserve the approved split policy; ask for missing assignments rather than inventing
them, and never use held-out test images for threshold calibration.

Prediction CSV columns are `image_name`, `instance_label`, `confidence`,
`bbox_x_tl`, `bbox_y_tl`, `bbox_x_br`, `bbox_y_br`. Retain sidecars identifying
checkpoint/source identity and settings with their CSV/checkpoint.

Evaluation and comparison require canonical basename identifiers. Keep standalone
`cy-predict image_name=name` and comparison `inference.image_name=name`. `stem` and
`path` are supported only for raw prediction export; they are not evaluated and must
not be passed to scoring. Unknown image identifiers fail before scoring. New hash-bound
prediction sidecars record `prediction_image_name`; historical files missing that field
are usable only when their identifiers satisfy canonical membership.

Dataset cache version 2 binds the CSV digest, ordered resolved media references and
format. Byte-identical relative-path CSVs in different directories may select different
images and therefore different entries. Identical absolute references can reuse an entry.
Hits do not hash image contents, inspect mtimes, copy images or repeat conversion.
Changing image bytes at unchanged paths still requires explicit idle cache invalidation;
old entries remain intact. Check source/installed version before relying on these fixes.

`CY_HOME` defaults to launch cwd. Explicit outputs, caches, logs and scratch must
remain inside it, including symlink destinations. SDK configuration and external
inputs retain their normal paths. Default durable results are under
`runs/<safe-project>/<safe-task>-<task-id>/`. Pipeline `run_dir` groups outputs;
dataset cache must be outside that run directory. Scratch under `.tmp/cy-runtime-*`
survives success/failure/cancellation; remove only task-owned inactive resources.

## Independent stages and model identity

Generate each needed command tree, for example:

```bash
cy-init-config predict-config --command cy-predict
cy-predict --config-dir predict-config --config-name cy-predict \
  ground_truth=./ground_truth.csv weights=./best.pt model_label=local-detector \
  ultralytics_predict.device=cpu fiftyone.enabled=false

cy-train ground_truth=./ground_truth.csv ultralytics.device=cpu
cy-val ground_truth=./ground_truth.csv weights=./best.pt \
  model_label=local-detector ultralytics_predict.device=cpu
cy-metrics ground_truth=./ground_truth.csv predictions=./predictions.csv \
  model_label=local-detector fiftyone.enabled=false
cy-report comparison_dir=./comparison
cy-ground-truth data_yaml=./data.yaml
```

Also pass the intended ClearML project, task and tags to standalone runs; add their
overrides or edit the generated command wrapper. A local checkpoint needs
`model_label` when source identity cannot be recovered. Don't fabricate that provenance.
Standalone comparison uses current images, defaults to test, and accepts explicit
ClearML references:

```bash
cy-compare ground_truth=./ground_truth.csv \
  baseline_model.task_id=BASELINE_TASK candidate_model.task_id=CANDIDATE_TASK \
  ultralytics_predict.device=cpu
```

Local comparison references require `source=local`, weights, a label when identity
is unavailable, and exact per-class thresholds. Retrieve those thresholds rather
than inventing defaults. Reports consume paired evaluated dashboards named by
`comparison_manifest.json`, not historical standalone dashboards.

## Other native models and export

Generate configuration for an already installed backend:

```bash
cy-init-config detector-config --command cy --model rfdetr/small
cy --config-dir detector-config --config-name cy \
  ground_truth=./ground_truth.csv model.train.epochs=10 \
  model.train.device=cpu model.predict.device=cpu
```

Select presets through `model=...`; native detector controls are `model.train.*`
and `model.predict.*`, and data is derived as COCO from the same CSV. Check installed
presets/help before selection. DEIMv2 requires its approved source checkout and
explicit initialization weights; these adapters support CPU/single GPU, not YOLO DDP.

`cy-export` supports Faster R-CNN only. Its distinct operations are CPU/FP32 ONNX,
GPU engine build and GPU verification. Generate `cy-init-config export-config
--command cy-export`, inspect `export.operation` and required inputs before executing.
ONNX needs weights; build needs `export.onnx`; verification additionally needs weights,
`export.engine` and labeled ground truth. Don't infer backend availability from config
generation or compose success.

## Direct execution and shared queue

Direct execution honors selected devices without admission, reservation or waiting.
Direct `cy -m` is sequential. Inspect installed `cy-queue --help` before using the
shared-configuration and archive interface; older installations require an upgrade
under the user's existing authorization, not invented compatibility flags.

In clearml-yolo 0.34.0+, every queue command except help requires a filled TOML
configuration. By default, each
user creates `$XDG_CONFIG_HOME/clearml-yolo/queue.toml` when `XDG_CONFIG_HOME` is
absolute and nonempty; otherwise use `$HOME/.config/clearml-yolo/queue.toml`.
Relative XDG values are ignored. If `HOME` is unavailable, the user's account home
supplies the fallback. Fill all three fields with the team's agreed locations:

```toml
[queue]
name = "Team training"
root = "/srv/clearml-yolo/queue"
coordination_root = "/var/lib/clearml-yolo/coordination"
```

These paths are portable examples, not discovered host settings. All paths must be
absolute. Metadata may use NFS with consistent UIDs. Coordination accepts supported
local Linux filesystems and Linux NFS/NFSv4 mounts whose effective superblock options
include `hard` and `local_lock=flock` or `local_lock=all`, with none of `soft`,
`softerr` or `nosharecache`. Default remote NFS locking is rejected. The queue needs
host-local process-lifetime locks; all queue users on one host must share the Linux
NFS client/inode cache, mount namespace, PID namespace and fixed local bootstrap.
Different hosts need distinct machine identities. Hard NFS I/O can block during a
server outage even though locks are local. Validate the actual target mount; a local
configuration check or modeled mount test does not establish remote NFS behavior.

Administrators provision shared-directory access; newly created roots use sticky
permissions even with a private umask, while existing roots/parents remain unchanged.
Snapshots stay owner-only, and private logs/progress stay in the submitting project.
An explicit absolute `CY_QUEUE_CONFIG` selects another file, including one shared by
all users. Set the override in each participating login and worker-service environment;
exporting it in one shell does not configure another user or service:

```bash
export CY_QUEUE_CONFIG=/etc/clearml-yolo/queue.toml
cy-queue config
cy-queue config --json
```

`config` validates and reports settings without creating queue state or binding a host.
Missing or incomplete settings fail without creating metadata or coordination roots.
There is no project-local fallback, generated configuration or implicit queue location.
`cy-queue --root /absolute/path ...` overrides only
metadata; legacy `CY_QUEUE_ROOT` must agree or the command fails. Host-wide binding
rejects conflicting coordination locations across users and queue roots. Initially
configure an existing stand's current location. Relocation requires all workers and
guardians stopped, uncertain jobs recovered, and offline administrator maintenance
preserving complete ownership/evidence and the protected binding. No automatic migration
or SQLite conversion occurs; never delete coordination records to bypass a conflict.

For authorized scheduled runs after configuration:

```bash
cy-queue submit --gpus 1 -- --config-dir cy-config --config-name cy
cy-queue worker
cy-queue list --json
```

Submit counts or exact `--devices 0,2`, never both. Submission integers use CUDA-visible
logical order, including `CUDA_VISIBLE_DEVICES` remapping. Remove native GPU selectors in queued
config: allocation supplies all training devices and first inference device; explicit
CPU phases stay CPU. Submission validates and freezes the entire sweep, returning stable
job numbers and task names. `submit --label "Training experiment"` overrides the
configured task name. Commands accept `job-17` and legacy raw job identifiers; numbers
remain stable across reorder, archive and restart. Add `--json` for automation: exact
machine identifiers remain available. Worker execution uses the owning user's environment and revalidates installation
identity. Changed installations require resubmission. Unsupported Linux ownership/locking
conditions fail explicitly; inspect status rather than deleting coordination files.

### Archive and readable queue views

```bash
cy-queue list
cy-queue archive job-17 job-18
cy-queue list --archived
cy-queue list --all --json
cy-queue restore job-17
```

Only owners archive or restore their jobs. Archive accepts completed, failed and
cancelled outcomes; pending, active and recovery-required jobs remain visible. Default
list/TUI views hide archived jobs. Owner-authored sidecars preserve execution records,
snapshots, logs and order history. Restoring does not rerun a job or change its outcome.
Repeated requests are idempotent. A storage error may leave earlier targets archived;
the error is surfaced and retrying the command is safe. A job later requiring recovery
reappears in the main view regardless of its stored archive flag. No archive operation
releases GPUs or provides process-cleanup proof.

Current `list`/TUI monitoring keeps a readable public summary when an owned job's
private snapshot or status is unreadable or invalid, including missing, malformed or
hash-mismatched snapshots. The affected row says
`Status unavailable; showing last recorded state`; healthy jobs continue refreshing.
Text and TUI output show a warning count. `list --json` adds `warning_count` and
`refresh_issues` entries containing `job_id`, readable `reference` and safe `message`.
Warnings apply only to jobs visible in Main/Archived/All, disappear on successful
refresh and do not replace durable outcomes. Messages exclude private configuration
and raw validation details. Check matching installed source/output before relying
on these fields; older installations can still fail the whole view.

Viewing old package identities and legacy fields does not import Hydra or evaluate
saved pipeline overrides. Monitoring neither recovers jobs nor changes outcomes or
releases reservations. Admission, execution and recovery retain strict validation;
viewability is no evidence that an old snapshot can run. Do not migrate or rewrite
old tasks to make them visible. Invalid shared metadata, queue settings and
coordination failures remain queue-wide errors; a later failed TUI refresh retains
previously displayed rows while reporting the error.

The optional TUI uses readable rows and a pending-job dropdown for `b`/`a` ordering.
`x` archives, `s` restores, `v` cycles Main/Archived/All and `o` opens the selected task.
Closing the TUI does not stop workers. Human output uses names, job numbers, physical
GPU indexes and word aliases; explicit JSON, provenance and URL targets retain exact
identifiers. Visible report banners/captions omit opaque identifiers and checkpoint
hashes, which remain in embedded metadata. Raw capture bytes remain available locally;
terminal and published console streams use readable formatting.

Automatic availability uses only used VRAM strictly below 2 GiB (2048 MiB); exactly
2 GiB is busy. This fixed limit is rechecked immediately before each workload launch.
Current queue allocations and explicit holds stay exclusive unless released; stale
unlocked records alone do not veto capacity. Process lists and utilization are not
availability criteria. The limit does not establish model/compiler memory fit.

### Training GPU handoff (clearml-yolo 0.30.2)

Check the installed version against its matching queue contract before relying on this
behavior. Training holds all allocated GPUs. After native training returns and unused
model memory is released, the job sends a bounded handoff request to its external
guardian. The guardian durably retains the first GPU, releases extra locks and updates
public allocation before acknowledging. Those extra GPUs may serve other eligible jobs
during inference/comparison; memory admission still applies. The first GPU remains
reserved through pipeline/descendant exit unless explicitly freed. Without a training-finished notification,
the full allocation stays reserved. After durable shrink, later record-write failure
or a lost acknowledgement does not restore released extras. A failed or unacknowledged
handoff stops before inference; cleanup/recovery follows the remaining durable claim.
It does not establish successful pipeline completion. The job itself manages no queue
GPU locks.

The external guardian also owns descendant cleanup outside ClearML task finalization.
Unrelated same-UID `/proc` access denials do not poison live scoped supervision. Later
supervisor cleanup uncertainty reports queue `recovery_required` and retains recovery evidence
without retroactively failing a completed successful ClearML task. Required computation
and upload failures still fail. Inspect the task and queue state separately; artifacts
alone prove neither successful required publication nor safe resource release. Owner
recovery keeps conservative global ownership proof, so inaccessible or ambiguous process
evidence can still block recovery. Manual GPU freeing is a separate capacity operation
that needs no process-cleanup proof. Do not delete coordination files.

Retired `queue.*`, `autogpu.*`, `auto_gpu.*` and old Hydra launcher controls fail with
migration guidance. Each new multirun adds one shared readable ClearML tag containing
UTC date/time and a random word phrase; queued snapshots retain that one group tag.

### Worker diagnostics (clearml-yolo 0.30.1+)

`cy-queue worker` logs at INFO to stderr by default: startup queue root, host and UID;
changes to the owner's job state, allocation, waiting reason, progress and errors;
and clean shutdown. An immediate heartbeat and another every 60 seconds report idle
or active status with job-state counts. Existing terminal history contributes to
counts without replaying each old job. A pending job's `waiting=` detail explains
admission delays; an idle heartbeat means there are no active jobs for this owner.

Check installed `cy-queue worker --help` before using the verbosity option:

```bash
cy-queue worker --log-level DEBUG
```

DEBUG adds a summary on every poll; INFO is the default, while WARNING or ERROR
reduces output. This option belongs after `worker`. These commands start a worker;
an explanation-only request does not authorize running them. Readable submission results
and explicitly selected `--json` remain on stdout, separate from worker diagnostics.

The queued pipeline's combined stdout/stderr is retained in the submitted project's
`.cy/queue-logs/<JOB_ID>.log`, not streamed into worker status messages. Read that
file for detailed workload output and inspect `cy-queue list --json` for current
job state. A heartbeat alone does not prove training, GPU use or publication success.

### Simplified physical GPU controls (clearml-yolo 0.31.0)

These commands describe the clearml-yolo 0.31.0 contract. Confirm installed help before
using them; the 0.29.0 controls had different release semantics. `cy-queue gpu` integer
selectors follow physical `nvidia-smi`/NVML order regardless of `CUDA_VISIBLE_DEVICES`.
Do not translate a physical operator index through the submission's CUDA-visible map.
Queue text, JSON, TUI, status logs and errors show physical indexes or word aliases
for absent devices. Older hexadecimal aliases remain accepted inputs. Never show full
GPU UUIDs or claim tokens in user-facing output.

```bash
cy-queue gpu list --json
cy-queue gpu disable 2
cy-queue gpu enable 2
cy-queue gpu free 2
cy-queue gpu release 2
```

Holds persist per Unix UID and apply across queue roots on the host. Disable prevents
new allocations while active work retains capacity. Enable removes only the caller's
hold. Free/release immediately clears the selected GPU's existing reservations and all
holds, including live reservations; other GPUs in the allocation stay reserved.

Accept the user's declaration that the GPU is free. No token, reason, force flag,
confirmation or extra memory/process check is required. Free/release sends no signals,
requires no access to the original queue root and leaves job outcomes unchanged.
Repeated freeing succeeds. Internal generation events fence earlier guardian writes,
handoff, recovery and cleanup so they cannot recreate a freed reservation or affect
a newer one. New jobs still need fresh memory admission below 2048 MiB: manual freeing
does not create a persistent bypass. Upgrade cooperating workers to the matching
protocol; an explanation-only request still authorizes no mutation or cancellation.

Remove `worker --max-used-memory-gib` from updated worker commands. Remove `--reason`,
`--claim-token` and `--force` from GPU controls. If installed help still requires the
old arguments or lacks `free`, report that version mismatch; do not invent a workaround
or silently upgrade the installation.

Version 0.29.0 also rejects missing Linux supervision capabilities such as `os.pidfd_open`
before native work, checks the snapshot interpreter before queue admission and reports
the affected interpreter. Verify its Python/kernel environment; do not add PID-only
cleanup or alter dependencies as a guessed fix. Resubmit when installation identity changes.

The opt-in `ddp_gpu` tests require multiple physical GPUs and real ClearML. Use the
matching repository's [GPU DDP guide](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/ddp-testing.md)
for exact options and evidence packaging. Offline/property or existing CPU/Gloo tests
cannot establish GPU/NCCL/publication success.

## Evidence and failure handling

`--help`/`--cfg job` avoid configured workload/task execution but may create scratch/logs;
check them with an isolated existing workspace selected by `CY_HOME`. Composition proves
schema compatibility, not input availability, model loading, GPU memory or publication.
Missing inputs remain `???` in generated config until supplied.

Metrics require validation data and `calibration_split=val`; other calibration splits
fail. Requested test metrics use those frozen thresholds. Compare models on identical
current images with compatible labels/settings. Record CSV/checkpoint identity, selected
splits, resolved inference/evaluation settings and output/task evidence before making
accuracy claims. Invalid prediction geometry is dropped from an evaluation copy; raw
CSV remains unchanged, and empty predictions count unmatched truth as false negatives.

Mandatory upload failure fails execution even when local files exist. Optional visualization
failure can leave successful computation. Report the failing operation and retained paths;
never change dependencies, credentials or shared services as an inferred repair.

## Retrieve version-sensitive details

Use installed help/configuration and matching source first. These maintained upstream
pages provide additional detail when access is available; compare their revision with
the installation before treating newer behavior as supported. The package examples above
remain usable without fetching these links.

- [Command/configuration contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/project-contracts.md)
- [Current contract index and implementation links](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/current-contracts.md)
- [Native YOLO configuration](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/005-explicit-detection-config/contracts/native-configuration.md)
- [Backend settings and prerequisites](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/multi-model-backends.md)
- [Shared queue contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/queue.md)
- [Python use-case imports and explicit dependencies](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/python-import-migration.md)
