---
name: cveta2
description: Use when operating cveta2 CLI or Python API to export CVAT annotations, fetch tasks, upload datasets, convert CSV/YOLO/COCO detection data, verify detection CSVs, prepare and explicitly publish reviewed dataset bundles, manage labels/tasks or mixed CVAT/S3 datasets with shared project mappings, or diagnose cveta2 configuration and cache failures. Excludes model training, accuracy evaluation, generic CVAT administration, and developing cveta2 itself.
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
   `CVETA2_CONFIG` for personal settings and `CVETA2_PROJECT_CONFIG` for the
   authoritative shared project file; `--config` is command-specific. Managed
   commands also accept `--project-config`. Never override shared policy from
   personal settings. Obtain credentials without printing them.
   For automation set `CVETA2_NO_INTERACTIVE=true` and provide required inputs.
4. Separate local operations from remote effects. Conversion and merge write
   local output; upload and task/label edits change CVAT. Fetch exports locally;
   `--skip-validation` changes eligible CVAT jobs. Bundle preparation is local
   review plus prerequisite reads; `upload-dataset` explicitly publishes to ClearML.
   Managed `dataset report-errors` creates CVAT issues and `dataset publish` may
   resume that feedback before publication. Preserve authorization for both effects.

## Execute and verify

Run the scoped operation and inspect exit status, skipped-task/image warnings,
incomplete-issue summaries, and resulting artifacts or remote IDs. Use
`CVETA2_RAISE_ON_FAILURE=true` when the request requires fetching every selected
task: otherwise some task-level
5xx errors can leave partial output. A successful exit alone does not prove
complete coverage. Explicit publication requires its own finalized dataset ID.

For CVAT failures, use the sanitized failed-request operation, endpoint, UTC and
available request ID to guide investigation. Gateway status suggests checking
proxy/ingress and backend availability; it does not identify the faulty component.
Retry attempt counts describe cveta2's outer budget, and summed worker time is
not elapsed time. Read the failure-reporting section in the usage reference.

Standalone fetch preserves blank `split` values; assign and validate `train`,
`val`, or `test` before conversion. Managed preparation instead owns stable
60/20/20 split targets with seed 42, configurable only in the shared definition.
Inspect shape types, dimensions, image
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

## Verify and publish reviewed bundles

The verification/bundle interfaces in [usage](references/usage.md#verification-and-reviewed-publication)
were introduced in cveta2 v0.11.0. Check installed help before using them; do not
claim they exist in an older tagged installation. Validate first without cleanup;
accept repairs or rejected-row removal only when requested. Splits are assigned
externally. Prepare all four fetch CSVs into a new directory, inspect diagnostics
and payload, then publish that prepared directory separately. Any changed input,
class/root/image prerequisite or newer parent requires preparation again.

## Managed source datasets

Read the [managed workflow](references/usage.md#managed-datasets-and-shared-project-configuration)
when combining projects or S3 annotation CSVs, attaching shared definitions,
migrating old configuration, or reporting verification errors to CVAT. Check
installed help before using this newer interface; an older tag may not contain it.

Setup attaches the shared file without editing it. Migration previews a split,
requires user verification, and preserves the original. Do not bypass missing or
conflicting shared policy with personal settings or silently approve migration.

Fetch checks task metadata before reusing published unchanged task annotations.
The user runs any transformation script externally. Preparation splits/verifies
its resulting CSV with source evidence preserved independently. S3 CSVs must
already have annotation columns; referenced images must be available locally.

Review cleanup and feedback outcomes before publication. Unmapped findings stay
local; failed mapped feedback blocks publication. Issues do not alter annotations
or job status. Publish exactly dataset.csv, dataset.yaml and tasks.yaml; the last
contains source snapshots, so no ZIP or image payload is needed.
