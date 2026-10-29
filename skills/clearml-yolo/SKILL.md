---
name: clearml-yolo
description: Configure and use clearml-yolo commands for CSV-based detector training, prediction, validation, metrics, comparison, reports, model export, native model-weight preparation, local training batch estimation and shared GPU queues. Use for clearml-yolo usage and configuration questions or authorized runs; exclude generic YOLO advice, service administration and standalone digital-metrics or report-generator usage.
---

# Use clearml-yolo

Choose the command that produces the requested result, prepare its actual inputs,
and verify local and published outputs separately. Use the available shell and
filesystem tools; this skill requires an existing clearml-yolo installation and
does not provide credentials or administer infrastructure.

## Establish inputs and effects

1. Inspect the installed command help, existing configuration, relevant source or
   maintained documentation, and existing outputs before changing anything. Read
   [usage](references/usage.md) for command selection and configuration examples.
   Preserve the user's chosen model, device, environment and dependency revisions.
2. Identify the local annotation CSV or ClearML dataset selector, accessible images,
   checkpoint or model references, requested stage/splits, output workspace, and
   ClearML project/task/tags. Ask only for essential
   missing inputs that cannot be recovered from context. Do useful inspection while
   resolving them; do not invent paths, model thresholds or credentials.
3. Separate configuration inspection from execution. `--help` and `--cfg job` display
   guidance or composed configuration without starting the configured workflow, but
   startup can create local scratch/logs. Use an isolated `CY_HOME` when checking examples.
   `cy-init-config` writes YAML. Experiment commands create a ClearML task, run their
   workload and publish results. The `cy-batch-estimate` interface, available from
   CY 0.41.0, runs local GPU trials without ClearML; inspect installed help before
   using it. Queue submission persists jobs; workers execute them.
   An explanation-only request authorizes neither a workload nor queue submission.
   Experiment commands also warn on stderr when the launch directory cannot be
   confirmed as a Git working tree, including during help/config inspection. This
   advisory does not block execution or initialize Git; see the launch-directory
   guidance in [usage](references/usage.md#launch-directory-and-git).

## Configure and execute

Generate a new command-specific tree with `cy-init-config DIRECTORY --command COMMAND`
and use `COMMAND --config-dir DIRECTORY --config-name COMMAND`. Preserve generated
`defaults`; replace required `???` inputs. Existing example files cause a collision;
use a new directory or deliberately requested `--force` after preserving custom values.

For custom Albumentations JSON, universal Mosaic, or preprocessing conflicts, read
[training augmentation](references/augmentation.md). Check that the installed version
exposes the profile before execution; preserve backend-owned final preprocessing.

For YOLO, put native keys at the root of `ultralytics/default.yaml` and
`ultralytics_predict/default.yaml`. Use `ultralytics.*` for training and
`ultralytics_predict.*` for inference overrides. CSV input owns dataset selection;
do not substitute `ultralytics.data` or raw `cfg` for wrapper inputs. Select other
backends through `model=...` and their `model.train`/`model.predict` controls after
checking that the chosen backend is already available. Model assets resolve during execution or an explicit
`cy-download-weights` call when supported by the installed version. Actual `cy` alone
attempts the eligible catalogue automatically once per catalogue/cache fingerprint;
completed partial failures require an explicit retry. Preserve custom architecture
YAML, pretrained values, resume and local selectors. Read
[model preparation](references/usage.md#model-assets-and-offline-recovery) for native
cache destinations, exclusions, inspection boundaries, proxies and offline recovery.

Read [batch estimation](references/usage.md#training-batch-estimation) for explicit
training `batch=null`, single-GPU measurements, explicitly extrapolated global
microbatches and strict source compatibility. In estimation explanations, include
the default output counts, fresh single-GPU confirmation, per-GPU ceiling versus
extrapolated totals, and target/source provenance. GPU count alone does not prevent
compatible extrapolation; dataset bounds and the separate physical DDP correctness
gate still apply. Check installed support: FCOS, native multi-GPU training, estimation
and self-contained exports are available from CY 0.41.0. Native YOLO fresh
confirmation and physical DDP acceptance remain incomplete; availability does not
establish those outcomes. Do not infer support from older installed packages.

For ClearML ground truth, use a dataset ID or project/name pair with an optional
version; read the source rules and SDK archive/cache caveat in
[usage](references/usage.md). Require only root `dataset.csv` in the finalized
dataset; additional files or linked entries are rejected. Images must remain
accessible through unchanged CSV paths. Local inputs still work with one warning.
Dataset resolution
occurs during execution, not configuration inspection or queue submission.
Default installations include detector, ONNX and TUI dependencies; do not suggest
removed project extras or change external source/system prerequisites.

All experiment stages require ClearML with remote artifact storage. There is no
`clearml.enabled=false` switch. FiftyOne is optional on commands exposing its group;
disable it explicitly when unnecessary. Keep project outputs and preparation state
inside `CY_HOME`; native model asset caches may follow SDK roots outside it. Use a
fresh run directory. External inputs remain valid.

Direct commands honor native devices; they do not wait for available GPUs. For
authorized queued work install the separate CY-specific `cy-queue` distribution after
CY in the same environment, then use its interface and inspect job state.
Read the queue section in [usage](references/usage.md): current queue commands require
filled per-user TOML settings, or an explicit `CY_QUEUE_CONFIG` shared-file override.
NFS coordination needs the documented mount and same-host locking conditions.
Use stable `job-N` references and readable task names;
archive/restore changes visibility without deleting results or releasing resources.
Never translate retired `queue.*` or `autogpu.*` controls into guessed settings.
For GPU holds or manual freeing, read the queue section in
[usage](references/usage.md) and check installed `cy-queue gpu --help`. Operator
integer selectors use physical `nvidia-smi` order; submission integers keep CUDA-visible
logical order. Show physical GPU indexes, never full GPU UUIDs or claim tokens.
Accept an explicit declaration that a GPU is free: use `gpu free INDEX` without
tokens, reasons, force flags, confirmation or additional memory/process checks. It
clears only that GPU's reservations and existing holds, preserves running processes
and job outcomes, and grants no persistent bypass of new-job memory admission.

## Verify and report

Inspect exit status, local files, checkpoint/prediction provenance, resolved settings,
and ClearML publication evidence. Metrics calibrate on `val`; comparisons use frozen
thresholds on the same current images. Do not claim native/GPU/publication success
from composition or mocked checks. Preserve diagnostics on failure; distinguish
missing inputs, configuration errors, workload failures and publication failures.

Return the selected command, meaningful configuration choices, readable output/task links,
checks actually performed and remaining limitations. For skill evaluation, read
[evaluation cases](references/evaluation.md); expected evidence is not a test result.
Use task/model names, queue job numbers and physical GPU indexes or word aliases in
user-facing explanations. Keep exact IDs and hashes in explicit machine output,
provenance, retained raw diagnostics and link targets.
