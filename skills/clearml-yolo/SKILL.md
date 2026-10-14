---
name: clearml-yolo
description: Configure and use clearml-yolo commands for CSV-based detector training, prediction, validation, metrics, comparison, reports, model export and shared GPU queues. Use for clearml-yolo usage and configuration questions or authorized runs; exclude generic YOLO advice, service administration and standalone digital-metrics or report-generator usage.
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
2. Identify the annotation CSV and images, checkpoint or model references, requested
   stage/splits, output workspace, and ClearML project/task/tags. Ask only for essential
   missing inputs that cannot be recovered from context. Do useful inspection while
   resolving them; do not invent paths, model thresholds or credentials.
3. Separate configuration inspection from execution. `--help` and `--cfg job` display
   guidance or composed configuration without starting the configured workflow, but
   startup can create local scratch/logs. Use an isolated `CY_HOME` when checking examples.
   `cy-init-config` writes YAML. Execution commands create a ClearML task, run their
   workload and publish results. Queue submission persists jobs; workers execute them.
   An explanation-only request authorizes neither a workload nor queue submission.

## Configure and execute

Generate a new command-specific tree with `cy-init-config DIRECTORY --command COMMAND`
and use `COMMAND --config-dir DIRECTORY --config-name COMMAND`. Preserve generated
`defaults`; replace required `???` inputs. Existing example files cause a collision;
use a new directory or deliberately requested `--force` after preserving custom values.

For YOLO, put native keys at the root of `ultralytics/default.yaml` and
`ultralytics_predict/default.yaml`. Use `ultralytics.*` for training and
`ultralytics_predict.*` for inference overrides. CSV input owns dataset selection;
do not substitute `ultralytics.data` or raw `cfg` for wrapper inputs. Select other
backends through `model=...` and their `model.train`/`model.predict` controls after
checking that the chosen backend is already available.

All execution stages require ClearML with remote artifact storage. There is no
`clearml.enabled=false` switch. FiftyOne is optional on commands exposing its group;
disable it explicitly when unnecessary. Keep project outputs inside `CY_HOME` and
use a fresh run directory. External inputs remain valid.

Direct commands honor native devices; they do not wait for available GPUs. For
authorized queued work use the separate `cy-queue` interface and inspect job state.
Never translate retired `queue.*` or `autogpu.*` controls into guessed settings.

## Verify and report

Inspect exit status, local files, checkpoint/prediction provenance, resolved settings,
and ClearML publication evidence. Metrics calibrate on `val`; comparisons use frozen
thresholds on the same current images. Do not claim native/GPU/publication success
from composition or mocked checks. Preserve diagnostics on failure; distinguish
missing inputs, configuration errors, workload failures and publication failures.

Return the selected command, meaningful configuration choices, output paths/task ID,
checks actually performed and remaining limitations. For skill evaluation, read
[evaluation cases](references/evaluation.md); expected evidence is not a test result.
