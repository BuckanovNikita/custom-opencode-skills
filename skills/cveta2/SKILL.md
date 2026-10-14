---
name: cveta2
description: Use when operating cveta2 CLI or Python API to export CVAT annotations, fetch tasks, upload datasets, convert CSV/YOLO/COCO detection data, manage labels/tasks, or diagnose cveta2 configuration and cache failures. Excludes model training, accuracy evaluation, generic CVAT administration, and developing cveta2 itself.
---

# Use cveta2

Produce the requested dataset, conversion, CVAT change, or diagnosis with evidence
of the actual result. Required inputs depend on the operation: project/task
selector, input dataset, output location, image source, and authenticated CVAT
configuration. This skill supplies usage guidance; authorization comes from the
user and host. Preserve explicit read-only, planning, and confirmation limits.

Read [usage](references/usage.md) before choosing arguments or interpreting
outputs. It covers CLI/API examples, configuration, CSV contracts, side effects,
and failure handling. Read [evaluation](references/evaluation.md) when testing
this package's discovery or task behavior.

## Choose and prepare

1. Identify the requested operation and inspect existing inputs/output paths.
   Resolve project/task IDs when names are ambiguous. A project selector can be
   an ID, name, `ORG/PROJECT`, or `/PROJECT` for the personal workspace.
2. Check the available installation using `cveta2 --help` and the selected
   command's `--help`. In an existing cveta2 checkout, use its documented
   `uv run` environment. If unavailable, report that and provide the applicable
   invocation; installation follows the user's scope.
3. Resolve configuration from the caller's environment or YAML. Use
   `CVETA2_CONFIG` for data commands; `--config` is command-specific. Obtain
   credentials through the existing secret mechanism without printing them.
   For automation set `CVETA2_NO_INTERACTIVE=true` and provide required inputs.
4. Separate local operations from remote effects. Conversion and merge write
   local output; upload and task/label edits change CVAT. Full fetch can publish
   to configured ClearML, and `--skip-validation` changes eligible CVAT jobs.
   For a requested export without those effects, specify `--no-clearml` and
   leave validation promotion disabled. Never infer mutation permission from
   an export request.

## Execute and verify

Run the scoped operation and inspect exit status, skipped-task/image warnings,
and resulting artifacts or remote IDs. Use `CVETA2_RAISE_ON_FAILURE=true` when
the request requires fetching every selected task: otherwise some task-level
5xx errors can leave partial output. A successful exit alone proves neither
complete coverage nor successful ClearML publication.

Fetch preserves blank `split` values; assign and validate `train`, `val`, or
`test` before CSV export to YOLO/COCO. Inspect shape types, dimensions, image
availability, and unique basenames/stems. Polygon export approximates contours
as bounding boxes; tracks are unsupported. Do not describe these outputs as
lossless segmentation data or measured model quality.

For interrupted upload, inspect the recorded task and resume state before
rerunning. Use `--resume` only with matching data/options; uncertain creation
must not become an automatic duplicate task.

Return the operation, artifact paths or CVAT task ID, relevant row/image counts,
checks performed, and partial-result warnings. Distinguish command preparation,
local verification, and live execution; report missing dependencies or access
as limitations rather than successful completion.
