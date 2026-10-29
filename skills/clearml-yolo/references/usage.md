# Command and configuration recipes

These examples assume the installed commands are available. Replace paths and run
identity with actual inputs. Commands without `--help` or `--cfg` execute real work.
Do not install packages or change dependency revisions as a usage side effect.

For custom training JSON and universal Mosaic, read [augmentation](augmentation.md),
including native-transform replacement, required final preprocessing and frozen replay.

## Installation contract

Check the matching installed package or source before relying on these interfaces.
CY's default runtime includes detector backends and ONNX/ONNX Runtime. The separate
`cy-queue` distribution owns the queue command, Textual UI and NVML dependency;
install it after CY in the same environment when queue functionality is needed.
Project extras `detectors`, `export` and `queue-ui` have been removed; do not recommend
package extras or `uv sync --extra` for those capabilities.
From a prepared source checkout, `uv sync --frozen --no-dev` installs runtime packages;
`uv sync --frozen` includes the default development group. Lower bounds, exact pins
and upper bounds are authoritative in the application's `pyproject.toml`; the
normal lock records selected versions, not proof of minimum-version compatibility.
Required upstream extras such as `rfdetr[train]` and `pandera[pandas]` remain.
Approved `digital-metrics` and `report-generator` source copies, detector dependencies
and TensorRT/CUDA deployment prerequisites still need their documented setup.
With `model.source_dir=null`, execution resolves the approved DEIMv2 source
automatically; an explicitly supplied validated checkout remains supported.
Do not substitute revisions or claim external dependencies were installed.

For custom training JSON and universal Mosaic, read [augmentation](augmentation.md),
including native-transform replacement, required final preprocessing and frozen replay.

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
| `cy-docs` | CY 0.43.0 local Russian reference, tutorials and bounded configuration previews; no tracking task |
| `cy-batch-estimate` | CY 0.41.0 local CUDA training-capacity trials; saved recommendations, no tracking task |
| `cy-dedup` | Optional cache directory; reflink maintenance, `--dry-run` previews |
| `cy-queue` | Submission/worker/state operations; separate durable scheduling |

Experiment stages require ClearML. The local batch estimator does not. Configure
credentials through the installation's normal SDK configuration; never put secrets
in examples or logs. `clearml.output_uri`
defaults to true; null, false and empty destinations are rejected. FiftyOne defaults
enabled on commands exposing that configuration group; `cy-val` has no such group.

For offline field search, effective defaults, YAML editing and composition previews,
read [local documentation](local-docs.md). `cy-docs` requires CY 0.43.0 or later;
check installed help before relying on it. It neither trains nor submits queue jobs.

## Launch directory and Git

`cy`, `cy-train`, `cy-predict`, `cy-val`, `cy-metrics`, `cy-report`, `cy-compare`,
`cy-export` and `cy-ground-truth` check the actual launch working directory once
per invocation, before Hydra can change it. This includes `--help`, `--cfg job`
and multiruns; individual pipeline stages or multirun jobs do not repeat the check.
`CY_HOME` selects the output workspace independently and does not select the
directory checked for Git membership.

Repository roots and subdirectories, linked worktrees, submodules and initialized
repositories without commits are valid working trees and produce no workspace
warning. No clean-tree, tracked-file, commit or remote requirement is imposed.
Bare repositories are not working trees.

If the launch directory is outside a working tree, or Git membership cannot be
verified because Git is unavailable, fails or times out, startup emits one advisory
warning on stderr and continues with the command's existing behavior and exit
status. Verification failure is not proof that the directory lacks a repository.
Keep stdout available for help or composed configuration. The warning recommends
versioning experiment configuration: launch from the intended Git checkout or
deliberately initialize a new repository with `git init` when that is the user's
choice. The application does not initialize or modify a repository, and the
warning alone does not authorize the agent to do so.

`cy-init-config`, `cy-dedup` and the separate `cy-queue` interface do not perform
this experiment-launch check. Check the installed version or source before
assuming that older installations have the warning.

## Full YOLO pipeline

```bash
cy-init-config cy-config
```

Fresh exports from CY 0.41.0 include the local command base (`pipeline.yaml`
for `cy`, `train.yaml` for `cy-train`), the selected model groups, both training
augmentation profiles and every application YAML referenced by their defaults.
Public YAML and private schema names are distinct, avoiding automatic schema matching
warnings. Existing trees remain usable without automatic rewriting. Keep local-file
precedence, interpolation, CLI overrides and validation intact; check installed behavior
before relying on the new layout.

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

## Ground-truth source selection

All existing `ground_truth` consumers accept a local CSV or a mapping selecting a
finalized ClearML dataset. This includes `cy`, `cy-train`, `cy-predict`, `cy-val`,
`cy-metrics`, `cy-compare` and optional `ground_truth` for `cy-export`.
`cy-ground-truth` still converts YOLO annotations through its separate `data_yaml` input.

```yaml
ground_truth: {dataset_id: DATASET_ID}
# Or:
# ground_truth: {project_name: annotations, name: ground-truth}
# ground_truth: {project_name: annotations, name: ground-truth, version: "1.2.0"}
```

Use the same mapping in a quoted CLI override:

```bash
cy-train 'ground_truth={dataset_id:DATASET_ID}' ultralytics.model=yolo11n.pt \
  ultralytics.device=cpu clearml.project_name=detection \
  clearml.task_name=dataset-train 'clearml.tags=[training]'
```

Replace the visible dataset placeholder and experiment identity before execution;
CPU is a real workload device. The dataset's `project_name` is independent of
the experiment's `clearml.project_name`. Select exactly an ID or a complete
project/name pair. `version` is accepted only with the pair; mixed selectors,
unknown fields and empty or incomplete values fail before lookup. Quote version
strings. Without a version, the SDK selects the latest available version
(highest semantic version when available, otherwise most recently updated).

The finalized dataset's current logical inventory must contain only root
`dataset.csv`; additional files or linked entries are rejected before download.
Lookup failure, an incomplete dataset or missing root CSV is an error; it does not
create a dataset. The selected and materialized input is the unchanged CSV. The SDK
may fetch whole archive chunks and populate its cache, including historically
removed media. Use a fresh CSV-only dataset lineage if transport must exclude that
media too. No image upload, conversion or path remapping is added. Absolute media
references remain absolute; relative references resolve beside the downloaded CSV.
Require accessible images at those paths.

Local `ground_truth=./ground_truth.csv` still works and emits one recommendation
to use a ClearML dataset during execution. Nested stages, including stages using
cleaned training CSVs, reuse the resolved source without another lookup or warning.
Help, configuration inspection/export and queue submission do not download datasets.
Queued jobs validate and preserve selectors; execution and remote replay resolve
the selector once in that invocation. Saved executable configuration keeps the
requested selector, with actual dataset identity, version and CSV digest recorded
separately. An unversioned selector can change between invocations; pin an ID or
explicit project/name/version when reproducibility needs a fixed dataset.

## CSV and path contracts

Training uses this header and absolute pixel corner coordinates:

```csv
image_name,image_path,instance_label,bbox_x_tl,bbox_y_tl,bbox_x_br,bbox_y_br,split
train.jpg,images/train.jpg,helmet,10,20,100,120,train
val.jpg,images/val.jpg,helmet,10,20,100,120,val
test.jpg,images/test.jpg,helmet,10,20,100,120,test
```

These rows describe existing images; they do not create fixture data. CSV image paths
resolve relative to the CSV directory; a local CSV argument resolves from command cwd.
For a ClearML source, the CSV directory is the downloaded dataset location.
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

`CY_HOME` defaults to launch cwd. Explicit project outputs, dataset caches, logs and scratch must
remain inside it, including symlink destinations. SDK configuration and external
inputs retain their normal paths. Native model assets alone may be published to
configured SDK caches outside `CY_HOME`; preparation state stays inside it. Default durable results are under
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
presets/help before selection. CY 0.41.0 adds native CUDA multi-GPU
training for DEIMv2, RF-DETR, Faster R-CNN and FCOS through a device list such as
`'model.train.device=[0,1]'`. Public batch is global across GPUs and must divide
evenly across ranks; accumulation separately determines the effective optimizer
batch. One supervised worker runs per GPU, the invocation owner exclusively publishes
to ClearML, and rank zero owns checkpoints/aggregate metrics. Prediction remains
single-device; queued inference keeps the first allocated GPU. Real multi-GPU
acceptance remains pending, so verify installed support before proposing execution.
Execution can resolve DEIMv2 source and requested pretrained assets automatically;
explicit source/weight paths remain authoritative and missing files fail.

The six FCOS selections available from CY 0.41.0 are `fcos/resnet50_fpn`, `fcos/resnet101_fpn`,
`fcos/convnext_tiny`, `fcos/swin_tiny`, `fcos/dinov2_small` and `fcos/dinov3_small`.
Each defaults to resolution 800, a pretrained backbone and newly initialized detector
head, with P3–P7 features and zero-based foreground labels. Preserve class zero;
Faster R-CNN keeps its separate internal background offset. FCOS shares CSV input,
training/prediction/validation/comparison, custom augmentation, checkpoint publication
and configuration export. Native GPU acceptance remains pending.

`cy-export` supports Faster R-CNN only. Its distinct operations are CPU/FP32 ONNX,
GPU engine build and GPU verification. Generate `cy-init-config export-config
--command cy-export`, inspect `export.operation` and required inputs before executing.
ONNX needs weights; build needs `export.onnx`; verification additionally needs weights,
`export.engine` and labeled ground truth. Don't infer backend availability from config
generation or compose success.

## Training batch estimation

This interface is available from CY 0.41.0. Check `cy-batch-estimate --help`
and the installed training schema first. Native YOLO fresh confirmation and physical
DDP acceptance remain incomplete; composing examples does not validate capacity or
training. Single-GPU FCOS smoke evidence does not establish full backend coverage.

```bash
cy-batch-estimate --imgsz 960
cy-batch-estimate --imgsz 960 --classes 150 --objects-per-image 300
cy-batch-estimate --imgsz 960 --models rfdetr/small,fcos/resnet50_fpn --devices 0,1
```

These commands execute local CUDA workloads without a ClearML task. Search once per
model, resolution, training profile and distinct GPU name/total-memory capacity.
Identical GPUs share one measurement. When a group needs measurement, use its first
idle GPU in the selected order; only that GPU must be idle. Preserve unrelated
workloads and reject interference on the probed GPU. Defaults cover 44 standard
pretrained YOLO variants and all native presets, 150 classes, 300 objects per image,
90% memory budget and a 1024-image **per-GPU** search ceiling.

`--devices` selects ordered hardware. `--gpu-counts` filters output counts, each over
the first N selected GPUs; the default is 1 through the selected count. It launches
no extra searches. `--memory-budget`, `--search-limit`, `--trial-timeout` and `--refresh`
control measurement; extrapolated global batches may exceed `--search-limit`.
The default trial timeout is 300 seconds. Preserve native precision/compilation and
maximum effective resolution, including multiscale. YOLO auto optimizer probes both
supported resolved paths and retains the smaller validated capacity.

Search grows exponentially then refines by binary search. Every trial uses one GPU
and performs five warm-up and ten measured optimizer updates plus bounded validation;
the winner is repeated in a fresh process on one GPU. There are no distributed probes
or multi-GPU confirmation runs. Confirmed CUDA OOM or memory-budget excess bounds
capacity; timeout, failure, nonfinite computation, missing assets and contamination
are separate failures. `ceiling_limited` remains usable without claiming an
unconstrained maximum; failed measurements cannot supply training batches.

The versioned atomic catalogue is `CY_HOME/.config/clearml-yolo/batch-estimates.json`.
It retains actual global/per-GPU measurements, diagnostics and provenance. Compatible
completed cases are reused; refresh invalidates matching success before reprobes.
Match architecture, nominal/effective size, GPU types/capacities, relevant versions,
precision, compilation, optimizers, accumulation, EMA and augmentation. First prefer a
compatible existing measurement for the exact ordered allocation/count, including
legacy records. Otherwise derive an explicitly `extrapolated` global recommendation:
N times the single-GPU capacity for identical hardware, or N times the smallest
compatible per-GPU capacity for mixed hardware. Require a compatible single-GPU source
for every type/capacity; never extrapolate across unmeasured hardware. The result is
divisible by N and remains workload-specific. It is not a measured multi-GPU maximum.
Retain source boundary/ceiling status. Derive recommendations during lookup/reporting;
never create synthetic trials or rewrite measured evidence.

Only explicit training null selects saved evidence; numeric defaults stay unchanged:

```bash
cy-train model=fcos/resnet50_fpn ground_truth=./ground_truth.csv \
  model.train.imgsz=960 model.train.device=0 model.train.batch=null \
  clearml.project_name=detection clearml.task_name=fcos-estimated 'clearml.tags=[training]'
```

For YOLO use `ultralytics.batch=null` with matching `ultralytics.model`, `imgsz` and
`device`. Lookup happens after dataset preparation and actual device selection. The
foreground vocabulary and maximum objects on any train/validation image must fit
inside the synthetic bounds; average density is insufficient. Missing, stale, failed
or incompatible sources stop training with an estimator command or explicit-batch
alternative. Recommendation provenance identifies measured/extrapolated origin,
target allocation, source keys, global/per-GPU values and source boundary/ceiling
status. Training retains normal failure behavior without automatic OOM retry or
hidden batch reduction. Preserve requested null in
canonical replay; resolved batch and estimate provenance are recorded separately.
Queue submission keeps null and the worker resolves it against assigned GPUs and its
accessible `CY_HOME` catalogue. Submission itself never probes. Custom augmentation,
CPU/MPS and unsupported/custom profiles require explicit batches when matching evidence
is unavailable. Prediction settings and existing YOLO AutoBatch behavior are unchanged.

## Model assets and offline recovery

Check installed command help/source before using this source feature with an older
release. `cy-download-weights` explicitly prepares the eligible catalogue, reports
native destinations and failures, and exits nonzero if incomplete. Rerun it to retry
unresolved assets; validated files are reused, and compatible legacy CY files may
be copied into native caches without consuming the originals. Preparation does not
install packages, create ClearML tasks or change approved dependency revisions.

```bash
cy-download-weights
```

### Pinned project weight archives

CY 0.44.0 adds pinned project archives to bulk preparation. Check matching
installed-version/source help and published-snapshot evidence before relying on this
feature; older installations do not gain it from a weights upload alone. Preparation first
reuses valid native assets, then tries the packaged `weights-*` GitHub Release pin
in `BuckanovNikita/clearml-yolo`, then ordinary upstream resolution. It never selects
a moving latest release. Standalone selected-model execution keeps its normal asset
resolution, and explicit local inputs remain authoritative.

Each independent tar archive and its selected members require byte-size/SHA-256
verification before atomic native publication. Unsafe paths, links, duplicate members
and special files are rejected. Failed or unavailable mirrors warn and retain upstream
fallback; asset identities absent from the pin also keep that route. Downloaded
archives are reused under `CY_HOME/.cache/clearml-yolo/weight-bundles/` with coordinated
locks. Interrupted transfers are retryable. Hub restoration retains the real commit,
requested revision reference, blob metadata and matching `config.json` without
upstream metadata requests, preserving native offline discovery and original-source
provenance. The mirror URL does not replace the upstream asset identity.

Weight snapshots have a separate release cadence from application versions; code
releases reuse the existing pin without republishing weights. New snapshots require
deliberate manifest updates and verification of actual hosted bytes. Packaging,
uploading or a source manifest alone does not prove offline native loading. Published
snapshot corrections use a new identity rather than replacing existing bytes.
Per-file licences remain attached: YOLO is AGPL-3.0 and DEIMv2 permits non-commercial
use and redistribution only. Preserve bundled licences, attribution and applicable
source directions; do not call the whole catalogue permissively licensed. The CY
model-assets exception does not authorize CY wheels, source distributions or registry
publication; this skill collection still follows its own Git-tag-only release policy.

### Catalogue, execution and native locations

The catalogue covers maintained detection YOLO names, RF-DETR, eligible Faster
R-CNN/FCOS backbones and DEIMv2 small/medium with pinned source. Bulk preparation
excludes DEIMv2 large/xlarge and `dinov3_small`; explicit selection retains existing
execution and access behavior. Shared backbones use a shared native cache entry.

Actual `cy` pipeline execution attempts this catalogue before model construction,
including with explicit local model weights. A completed attempt is remembered per
catalogue/asset-identity/native-destination fingerprint, including partial failures.
The pinned-mirror implementation also fingerprints the bundle manifest;
changing the pin triggers a new attempt.
Automatic failures warn and continue; the selected model keeps normal error handling.
Use the explicit command to retry failures. Changed identities/destinations trigger
another automatic pass. Concurrent starts coordinate; interruption leaves the pass
unfinished. Receipts and locks live under
`CY_HOME/.cache/clearml-yolo/preparation/`. A receipt proves an attempted pass, not
complete offline availability.

Help, configuration inspection/export, queue submission and standalone stages such
as `cy-train` never run bulk preparation. A queue worker executing `cy` follows its
pipeline policy. Stage execution may still fetch selected-model assets; inspection
is not permission to download an entire catalogue. Package imports remain inert.

Ultralytics uses effective `weights_dir`, including normal aliases and auxiliary AMP
lookup. Torchvision URL backbones use `torch.hub.get_dir()/checkpoints`. timm follows
its installed pretrained configuration: HF assets require native repository/revision
metadata and snapshot discovery in the effective Hub cache, not a loose weight file.
HF readiness includes valid native `config.json` for the weights' actual snapshot
commit. Retry repairs missing/invalid config while retaining valid weights; the
weight file alone is insufficient. RF-DETR uses its configured model cache.
SDK roots/settings/selectors stay unchanged;
only native model assets, publication locks/temporary files and HF metadata may be
written outside `CY_HOME`. Application outputs, datasets, logs, scratch and preparation
state remain contained. DEIM weights/source retain managed paths:

```text
CY_HOME/.cache/clearml-yolo/models/<backend>/<preset>/<asset-id>/<filename>
CY_HOME/.cache/clearml-yolo/sources/DEIMv2/<revision>.tar.gz
CY_HOME/.cache/clearml-yolo/sources/DEIMv2/<revision>/
```

The approved source archive is checked against packaged code/configuration hashes;
explicit checkouts retain validation. Do not edit pinned source to pass validation.
Resolved DEIM paths enter execution copies only. Preserve original selectors,
including nulls, in canonical replay; `model_assets` records resolved paths, original
URLs and hashes separately. Explicit non-YOLO weights/backbones remain authoritative.
Requested pretrained initialization is not replaced by random or detector weights.

Keep custom YOLO architecture YAML, boolean/filename `pretrained`, local paths,
resume and Hydra overrides intact. Do not infer weights from a custom YAML basename
or substitute the architecture. Relative paths follow the consuming process's cwd.
Non-null `ultralytics.cfg` remains unsupported.

Bulk preparation with the mirror uses valid native assets, then the pinned
project archive, then ordinary upstream resolution. That upstream route uses validated
cache data before configured proxy and upstream requests.
`GHR` is a GitHub release repository root: preserve the entire
`owner/repository/releases/download/tag/filename` suffix, including for the project's
weight-release archives. `GH` is an Artifactory VCS
repository root for pinned DEIM `downloadCommit`, not a Git clone endpoint.
`HF_ENDPOINT` selects an HF endpoint, including an Artifactory Hugging Face API
endpoint; `HF_TOKEN` or standard SDK token configuration provides endpoint-scoped
authentication. Do not send credentials to unrelated origins or expose them in
examples/reports. Missing/empty proxies select upstream; other sources retain URLs.
URL construction or an HTTP transfer alone does not establish native offline loading.

### Proxy setup after a failed download

Set network, credentials and endpoint variables before starting a new CY process;
HF libraries can capture endpoint settings during import. For complete project
archives, use a Generic Artifactory remote repository whose upstream is
`https://github.com`:

```bash
export GHR="https://artifactory.example/artifactory/github-releases"
```

Replace the example host and key with administrator-provided values. A request for
`https://github.com/owner/repo/releases/download/tag/file.tar` retains its full path
under `GHR`, yielding
`https://artifactory.example/artifactory/github-releases/owner/repo/releases/download/tag/file.tar`.
Do not use a filename-specific URL or GitHub API root. See the
[remote repository protocol](https://docs.jfrog.com/artifactory/docs/remote-repositories).

For `GH`/`GHR` authentication, configure the actual Artifactory host in a private
per-host `.netrc` entry using approved credential storage; keep owner-only file
permissions (`chmod 600 ~/.netrc`). Requests uses that entry. `HF_TOKEN` is only for
the selected Hugging Face endpoint, not these Generic/VCS downloads. Keep secrets
out of URLs, configuration examples, command history and reports.

Optional original-source fallback routes are:

```bash
export GH="https://artifactory.example/artifactory/github-vcs"
export HF_ENDPOINT="https://artifactory.example/artifactory/api/huggingfaceml/hf-remote"
```

`GH` derives
`https://artifactory.example/artifactory/api/vcs/downloadCommit/github-vcs/owner/repo/commit?ext=tar.gz`;
it is the [VCS commit API](https://docs.jfrog.com/artifactory/reference/downloadvcscommit),
not Git clone access. Complete project bundles already include DEIM source through
`GHR`, so `GH` is optional unless original-source fallback is needed. Use `GH`;
`CH` is not an alias. The optional HF route requires the exact
[Hugging Face API endpoint](https://docs.jfrog.com/artifactory/docs/use-hugging-face-with-jfrog-cli),
not a Generic root. Load `HF_TOKEN` securely from a secret manager or protected SDK
token file. If using an interactive Bash prompt, the value need not appear in history:

```bash
read -r -s -p 'Hugging Face endpoint token: ' HF_TOKEN
export HF_TOKEN
```

Do not send the endpoint credential to unrelated public-origin fallback requests.
For an organization CA, set `SSL_CERT_FILE` and/or `REQUESTS_CA_BUNDLE` to its real
bundle path before launching; keep TLS verification enabled. Then rerun
`cy-download-weights`, retaining valid native caches. A completed automatic partial
receipt does not prevent this explicit retry.

Treat 401 as missing/invalid authentication and 403 as read-permission/access policy
failure. For 404, inspect the root, repository key and preserved upstream path, then
upstream availability or blocking; do not assume the weights are absent. Size,
checksum or format failures require a corrected proxy/cache response and retry,
not disabled verification, incompatible replacement weights or removal of valid
caches. Protocol-fixture results prove client routing/auth boundaries and fallback,
not live Artifactory connectivity, permissions or cache state. An explanation-only
or fixture-only request authorizes no live stand check.

On failure, preserve reported selections and destinations. Selected-model missing
asset errors name original URLs, exact local destinations and applicable override
or access requirements. Copy the exact file or DEIM archive there and retry; a prior
online receipt is unnecessary. Keep native HF metadata/snapshots intact. Gated
backbones require upstream access; do not substitute an incompatible checkpoint.

## ClearML Agent execution

For an externally managed Agent, the recorded bare `cy`/`cy-train` module entrypoint
restores one detached canonical configuration before required-input validation and
backend selection, initializes that runtime, then joins the same task. No queue package,
queue TOML or GPU reservation is needed. Keep Agent CUDA visibility and native logical
devices unchanged. This adds no submission command or dependency installer.

An absent canonical `run` object permits complete explicit launch inputs. A present
empty or malformed object fails; do not delete it to hide the error. Canonical settings
and supported scientific General overrides retain precedence, while original source
script metadata and task publication ownership remain intact.

## Direct execution and shared queue

Install CY from an available source checkout first, followed by the separate queue
checkout into that same Python environment; neither distribution is assumed published
on PyPI. Preserve the approved external dependency sources. For a coordinated offline
upgrade, stop all workers and drain/recover owned jobs before replacing either package.
Preserve queue records/history and shared lock/configuration locations. Pending jobs
must be resubmitted after installation identity changes; do not rewrite saved identities.

`cy-queue submit --command pipeline` is the default. Use `--command train` before `--`
for standalone training, followed by its existing CY Hydra arguments. Both require
explicit queue GPU requests and preserve owner/cancellation/recovery/handoff rules.

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

### ClearML parameters and cached pipeline steps

Check that the installed source exposes
`clearml_yolo.adapters.clearml.cache.normalize_task_hash` before using this interface;
older installations retain their existing behavior. Check matching source/help before
relying on the startup publication behavior below.

The CLI publishes a sanitized JSON `run` Configuration Object containing executable
inputs after YAML composition, CLI overrides, queue device binding and supported remote
overrides. It includes only the selected backend's active settings: YOLO uses
`ultralytics`/`ultralytics_predict`; alternative detectors use `model.train` and
`model.predict` without inactive YOLO defaults. Later result provenance does not replace
configured values, and observed native normalization is recorded separately. Embedded
application callers must explicitly supply or record complete inputs within their
invocation; direct use-case calls do not construct the full snapshot.

The separate JSON `run_overrides` Configuration Object shows only differences from the
built-in defaults for the same command and selected model preset. It includes YAML and
CLI changes and omits generated output routing, result provenance and inactive backend
groups. A resolved device binding remains when it differs from the selected defaults.
Both Configuration Objects are published during startup.
Nested unchanged values and empty parent mappings are pruned; meaningful `false`, zero,
null, explicit empty collections and ordered-list differences remain. It is a sanitized,
informational view: use full `run` for execution, cloning and replay. The recognized-task
cache projection excludes `run_overrides`; editing this derived view is not a way to
change execution or scientific cache identity.

Training `General` is published after supported remote overrides and parameter validation,
before dataset preparation and training. Native `General` omits `device`, `project`,
`name`, `save_dir`, `exist_ok` and CSV-generated `data`. Native lifecycle callbacks can
refresh observed normalization while retaining the configured `run` values. This applies
to direct and queued training and supported detector backends; the invocation owner alone
publishes in DDP. Commands and pipelines that skip training do not create training
parameters. Startup visibility does not establish that training has succeeded.
Remote YOLO training replay restores the actual device and full settings, applies scientific
`General` overrides and retains current output routing. New snapshots use JSON;
historical HOCON remains readable for replay. Do not delete replay settings to influence
cache lookup or change the native device requested by the user.
Legacy remote YOLO tasks with absent `run` or an omitted `ultralytics` group recover
their execution settings from General into the new full snapshot. Present groups,
including empty mappings, retain authority; current output routing stays protected.
Filtering new connection inputs does not delete General keys inherited by clones of
historical templates. Canonical replay and the cache helper still handle them;
parameter-history cleanup is outside this publication behavior.

For an existing external task-based ClearML controller, register before creating cached
jobs and after any framework callback registration:

```python
from clearml.automation.job import ClearmlJob
from clearml_yolo.adapters.clearml.cache import normalize_task_hash

ClearmlJob.register_hashing_callback(normalize_task_hash)
```

Use its existing `cache_executed_step=True` option. If the controller already registers
a callback, explicitly compose that callback's result with `normalize_task_hash` instead
of replacing it. Registration belongs in the controller process: a worker starts after
cache lookup. Importing/calling the helper alone installs no callback or pipeline runner;
the helper returns a detached hash representation without filesystem/network effects.

Pass `run` overrides as JSON text, for example
`configuration_overrides={"run": json.dumps(full_run_config, ensure_ascii=False)}`
after importing `json`. Keep the complete input mapping. The installed SDK converts
dictionary overrides to JSON with default ASCII escaping; explicit text preserves
Unicode through its replay parser. Dictionary overrides are otherwise normalizable.

For supported native YOLO training, `[0,1]` and `[2,3]` normalize as two CUDA devices.
Device count/type, unresolved selectors and forms rejected by the consuming backend
remain distinct. Do not apply YOLO's permissive aliases to strict detector/export
settings; inference with an unknown checkpoint backend uses a conservative common
subset. Known run/output names and output/cache roots are
ignored; datasets, checkpoints, model/backend, class names, seeds, batch, precision,
optimizer settings, augmentation, thresholds and enabled stages remain significant.
SDK-supplied code/container identity and unknown fields are preserved. Generated dataset paths normalize only
with recorded `run.prepared_dataset.cache_identity` and `root`; class mappings and split
definitions still contribute. Never discard arbitrary `name`/`path` fields or normalize
unproven input paths by basename. Unknown, malformed and historical HOCON Configuration
Objects remain unchanged and can prevent reuse conservatively.

Retain the ClearML project when it supplies a comparison model's fallback input
lookup, and pipeline run roots when skipped stages consume existing files there.
Pin concrete source task IDs before caching mutable latest-task/tag lookups; the
helper does not resolve them. JSON with active escapes whose meaning differs in
the SDK replay parser also remains unnormalized; literal Unicode and escaped
backslashes remain supported.

A hit reuses the previous task and its remote artifacts/models; it does not create files
at a new local destination. Historical tasks are not rewritten. Existing source-image
immutability and cache-invalidation requirements remain; normalization does not verify
file contents. Request real miss/hit/scientific-change evidence before claiming caching
works. Source inspection or synthetic GPU selectors do not prove physical GPU equivalence.

### Complete artifact publication

Only completed files are uploaded under their existing canonical artifact names. There
are no intermediate replacement uploads. Inspect the task while it is running when
verifying timing; a final artifact list alone cannot prove early visibility.

| Publication | First complete publication point |
| --- | --- |
| `gt_csv` | Effective ground truth has been validated and its canonical CSV written |
| Validation threshold CSV | Calibration and the complete CSV write finish |
| Evaluation/comparison dashboards | Each model/split evaluation and workbook annotation finish, before the next evaluation or comparison statistics |
| `predicts_csv` | All enabled prediction/evaluation contributors finish adding or enriching rows |
| Comparison/report workbooks | Each final annotated workbook is complete |
| Export artifacts | Each graph, engine, metadata document or corpus manifest is finalized; verification reports wait for all cases and corpus checks |
| Best Output Model | Training selects and finalizes the best checkpoint; existing upload verification still applies |

The workflow declares its enabled `predict`, `metrics` and `compare` result contributors.
Each signals completion after its last row mutation; the last completion assembles and
uploads the combined `predicts_csv` once, before later statistics, reports or optional
publication. Standalone commands declare their own contributors; standalone validation
includes prediction and metrics. A skipped comparison explicitly completes without
inventing rows. An empty contributor set requires no prediction artifact. Effective
ground truth is published once: identical repeat registration is harmless, while
conflicting ground truth fails.

Finalization verifies contributor completion and required uploads. A failure or
cancellation before the last contributor finishes leaves the incomplete aggregate local;
already completed uploads remain available after a later failure. Their presence alone
does not establish a successful invocation. Required upload rejection or final flush
failure still fails the task. A complete export verification report that fails its quality
gate is published before failure; an unfinished report remains local. Preserve diagnostics
and use task status plus downloaded artifact contents for final verification.

### Verify execution outcomes

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
- [Native YOLO configuration](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/000-baseline/spec.md#native-configuration)
- [Backend settings and prerequisites](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/multi-model-backends.md)
- [Shared queue contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/queue.md)
- [Stable parameters and controller cache contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/000-baseline/spec.md#stable-experiment-parameters-and-controller-caching)
- [Required publication and task lifecycle](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/000-baseline/spec.md#required-publication-and-task-lifecycle)
- [Python use-case imports and explicit dependencies](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/python-import-migration.md)
