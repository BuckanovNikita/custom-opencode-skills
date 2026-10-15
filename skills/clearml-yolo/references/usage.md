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
Direct `cy -m` is sequential. For authorized scheduled runs:

```bash
cy-queue submit --gpus 1 -- --config-dir cy-config --config-name cy
cy-queue worker
cy-queue list --json
```

Submit counts or exact `--devices 0,2`, never both. Submission integers use CUDA-visible
logical order, including `CUDA_VISIBLE_DEVICES` remapping. Remove native GPU selectors in queued
config: allocation supplies all training devices and first inference device; explicit
CPU phases stay CPU. Submission validates and freezes the entire sweep, returning stable
IDs. Worker execution uses the owning user's environment and revalidates installation
identity. Changed installations require resubmission. Unsupported Linux ownership/locking
conditions fail explicitly; inspect status rather than deleting coordination files.

### Training GPU handoff (clearml-yolo 0.30.2)

Check the installed version against its matching queue contract before relying on this
behavior. Training holds all allocated GPUs. After native training returns and unused
model memory is released, the job sends a bounded handoff request to its external
guardian. The guardian durably retains the first GPU, releases extra locks and updates
public allocation before acknowledging. Those extra GPUs may serve other eligible jobs
during inference/comparison; memory admission still applies. The first GPU remains
reserved through pipeline/descendant exit. Without a training-finished notification,
the full allocation stays reserved. After durable shrink, later record-write failure
or a lost acknowledgement does not restore released extras. A failed or unacknowledged
handoff stops before inference; cleanup/recovery follows the remaining durable claim.
It does not establish successful pipeline completion. The job itself manages no queue
GPU locks.

The external guardian also owns descendant cleanup outside ClearML task finalization.
Unrelated same-UID `/proc` access denials do not poison live scoped supervision. Later
supervisor cleanup uncertainty reports queue `recovery_required` and quarantines capacity
without retroactively failing a completed successful ClearML task. Required computation
and upload failures still fail. Inspect the task and queue state separately; artifacts
alone prove neither successful required publication nor safe resource release. Owner
recovery keeps conservative global ownership proof, so inaccessible or ambiguous process
evidence can still block it. Do not bypass that proof or delete coordination files.

Retired `queue.*`, `autogpu.*`, `auto_gpu.*` and old Hydra launcher controls fail with
migration guidance. Each multirun adds one shared random `multirun-...` ClearML tag.

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
an explanation-only request does not authorize running them. Submission IDs and
`list --json` remain on stdout, separate from worker diagnostics.

The queued pipeline's combined stdout/stderr is retained in the submitted project's
`.cy/queue-logs/<JOB_ID>.log`, not streamed into worker status messages. Read that
file for detailed workload output and inspect `cy-queue list --json` for current
job state. A heartbeat alone does not prove training, GPU use or publication success.

### Physical GPU controls (clearml-yolo 0.29.0+)

These interfaces are available from clearml-yolo 0.29.0; confirm installed help
before using them. `cy-queue gpu` integer selectors follow physical `nvidia-smi`/NVML
order regardless of `CUDA_VISIBLE_DEVICES`. Full GPU
UUIDs also work and are preferable after hardware/order changes. Do not translate a
physical operator index through the submission's CUDA-visible map.

```bash
cy-queue gpu list --json
cy-queue gpu disable 2 --reason 'Maintenance'
cy-queue gpu enable 2
cy-queue gpu release 2 --claim-token TOKEN_FROM_LIST --reason 'Reviewed stale allocation'
```

Holds persist per Unix UID and apply across queue roots on the host. Disable prevents
new allocations while active work retains capacity. Enable removes only the caller's
hold; other users' holds and quarantine remain. Upgrade all cooperating workers before
controls are used: a persistent compatibility fence rejects old admission protocols.

Release requires the current token from `gpu list`, a reason and ownership. It releases
the entire claim, not just the selected GPU, marks the job failed and retains audit/history.
Normal release performs verified cleanup. Add `--force` only for an explicitly requested
operator override of inaccessible stale evidence: force never signals and refuses held
locks, confirmed live guardians or owned descendants. The original queue root must be
accessible. Preserve holds, never delete coordination files, and reread status after a
stale-token refusal. Inspect before proposing recovery; an explanation request does not
authorize release or cancellation.

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
