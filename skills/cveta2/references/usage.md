# cveta2 usage reference

Use the installed command's help to reconcile version differences. Examples
assume supplied inputs and authorized operations; example paths are portable
placeholders. Optional deeper references are the upstream
[CLI guide](https://github.com/BuckanovNikita/cveta2/blob/main/docs/cli.md),
[configuration guide](https://github.com/BuckanovNikita/cveta2/blob/main/docs/configuration.md),
[Python API](https://github.com/BuckanovNikita/cveta2/blob/main/docs/python-api.md),
and [dataset contract](https://github.com/BuckanovNikita/cveta2/blob/main/specs/baseline.md#dataset-format-and-fetch).
These follow the default branch; installed-version help takes precedence for flags.

## Installation and configuration

Current cveta2 requires Python 3.12 or 3.13 (`>=3.12,<3.14`). Install from a trusted Git host and selected
version tag; no public package registry availability is assumed:

```bash
uv add 'git+ssh://git@<host>/<org>/cveta2@vX.Y.Z'
```

Run `cveta2 setup` interactively when configuring credentials is authorized.
Otherwise supply `CVAT_HOST`, `CVAT_USERNAME`, `CVAT_PASSWORD` through the caller's
secret mechanism; optional `CVAT_ORGANIZATION` sets the default organization.
Do not log credential values. Default YAML location is
`~/.config/cveta2/config.yaml`; `CVETA2_CONFIG` selects another file.
Connection precedence is explicit API connection fields, environment, YAML,
then built-in preset. `--config` is available on setup/setup-cache,
rather than being a global flag on data commands.

Portable YAML skeleton (credentials supplied separately):

```yaml
cvat:
  host: https://cvat.example.org
  organization: team
image_cache:
  Project name: /data/images
network:
  s3_workers: 16
  cvat_workers: 4
  retry_attempts: 5
```

Use project ID `123`, name `Project name`, `team/Project name`, or `/Project name`.
Duplicate names require IDs. Task names need a project; numeric `fetch-task` IDs
can infer their project. Explicit `-p` keeps automation unambiguous.

`CVETA2_NO_INTERACTIVE=true` replaces missing-input prompts with errors and can
allow existing output to be overwritten without prompting. Inspect destinations
first. Image downloads need configured image cache or `--images-dir`; choose
`--no-images` for annotation-only work. `setup-cache` configures image paths.

## Fetch and inspect output

```bash
export CVETA2_NO_INTERACTIVE=true
export CVETA2_RAISE_ON_FAILURE=true
cveta2 fetch -p 123 -o export --no-images
cveta2 fetch-task -p 123 -t 456 789 -o selected --no-images
cveta2 whats-new -p 123 --dataset export/dataset.csv
```

Full fetch produces:

| Artifact | Meaning |
| --- | --- |
| `dataset.csv` | Latest completed task's rows for each non-deleted image |
| `obsolete.csv` | Older completed data and data superseded by deletion |
| `in_progress.csv` | Unfinished task data |
| `deleted.csv` | Deletion records in the shared CSV schema |
| `raw.csv` | Additional full unpartitioned rows with `--raw` |

`fetch-task` produces unpartitioned `dataset.csv` and `deleted.csv`; it has no
`--raw` or `--skip-validation` option. `--completed-only` filters
task selection. Completion requires every job to be acceptance/completed;
highest task ID determines recency and deletion wins same-task ties.

`--skip-validation` on full fetch explicitly mutates CVAT: a non-ignored task
with a nonempty full job list qualifies only when every job is
annotation/completed or acceptance/completed. It promotes annotation/completed
jobs to acceptance/completed after strict cache invalidation, then refreshes
selection. Failures abort export; successful prior changes persist without
rollback. It is disabled by default and requires authorization for that change.

Fetch no longer automatically publishes to ClearML. Legacy automatic mappings,
`CVETA2_CLEARML`, `setup-clearml`, `--no-clearml` and `publish_clearml` are removed
without compatibility shims in cveta2 v0.11.0. Existing
user configuration is not automatically rewritten. Check installed help for the
version in use rather than passing removed controls.

Task annotation cache: normal fetch reads/updates, `--force` refreshes,
`--no-cache` disables reads/writes. `CVETA2_DISABLE_CACHE=true` also disables it.
Image cache is separate; `--save-tasks` retains intermediate per-task CSVs.

## CSV and conversion contracts

Row discriminator `instance_shape` is `box`, `none`, or `deleted`. Boxes use
pixel `bbox_x_tl`, `bbox_y_tl`, `bbox_x_br`, `bbox_y_br`; text labels preserve
literal identifiers, attributes are JSON, `image_path` is absolute when present,
and `s3_image_path` retains the S3 key. Distinct images need unique basenames and
stems across directories/splits. Repeated rows for one image are normal.

Rectangles retain rotation; standalone polygons become tight axis-aligned boxes
with rotation zero and warnings. Invalid polygons, tracks, and unsupported
shape types are skipped. This is detection data, not preserved polygon contours.

Fetch leaves `split` empty. Before either export, assign each retained image to
`train`, `val`, or `test` using the requested split policy; do not invent a split
ratio. Validate dimensions, finite positive-area boxes, and image availability.
Missing images produce warnings, so compare expected images to produced files.
For a clearml-yolo handoff, supply the converted dataset and available image
files with validated split assignments. Fetch partitions describe task completion
and recency; they are not ML train/validation/test splits.

```bash
# split.csv already has validated split assignments and available images.
cveta2 convert --to-yolo -d split.csv -o yolo --image-dir /data/images --link-mode copy
cveta2 convert --to-coco -d split.csv -o coco --image-dir /data/images --link-mode copy
cveta2 convert --from-yolo -i yolo -o imported.csv --read-all-sizes
cveta2 convert --from-yolo -i predictions -o predictions.csv \
  --names-file classes.yaml --image-dir /data/images --read-all-sizes
```

YOLO export emits `dataset.yaml`, split images and label files, zero-based class
IDs and normalized center/width/height. COCO emits split images and
`_annotations.coco.json`, one-based category IDs and pixel x/y/width/height;
`val` becomes directory `valid`. Export returns its directory; YOLO import
returns the CSV path. There is no COCO-to-CSV command.

YOLO import uses dataset mode when `dataset.yaml` exists, otherwise prediction
mode with class mapping (`classes.yaml` accepts `{names: {0: person}}` or
`{0: person}`). Predictions may include confidence. Default import reads the
first image's dimensions for all images; use `--read-all-sizes` for varied sizes.
`auto` image placement attempts reflink then copy; other modes are explicit.
Repeated export can replace/prune exporter-owned output; inspect the destination.

## Upload, labels, and task changes

```bash
cveta2 upload -p 123 -d export/dataset.csv --labels all \
  --name imported --image-dir /data/images
cveta2 labels -p 123 --list
cveta2 ignore --list
cveta2 merge --old old.csv --new new.csv --deleted export/deleted.csv -o merged.csv
```

Upload needs project S3 cloud storage, images, and compatible project labels;
it uploads images, creates a CVAT task, and transfers annotations. Noninteractive
upload needs explicit `-p`, `--name`, `--labels`. A label filter selects frames
and retains co-occurring labels; `--labels all` includes unannotated images.
`--in-progress` excludes names from a supplied CSV. `--complete` changes jobs
to completed; `--mark-all-deleted` marks all uploaded images deleted.

For interruption, reuse matching data, labels, name, and options with `--resume`.
Current resume manifests validate intent and server state; old or mismatched
state requires investigation. Do not delete manifests or create another task to
hide uncertain writes. Capture task ID, URL, and returned counts.

Only `issue_state=new` with nonempty text and bbox coordinates creates upload
issues; exported open/resolved issues are not recreated. Split is ignored on
upload. CLI labels editing is interactive; for prompt-free edits use authorized
`cveta2.update_labels(...)` with explicit label IDs. `task mark-deleted`,
`drop-label`, `delete`, and `status` change CVAT; inspect their command help and
target IDs. Noninteractive drop-label/delete require `--yes`. API state uses
`"in progress"`; CLI uses `in-progress`.

Merge defaults to new input winning per image; `--by-task` uses task ID.
`s3-sync` downloads project images; scope with `-p` and inspect configured target
roots before running. Its CLI `-p` selects a configured project name rather than
offering the general selector behavior of fetch. `doctor` diagnoses configuration/cache; `--cache`
changes permissions. Required check failure exits 1; optional warnings alone exit 0.

## Python API and failure reporting

```python
import cveta2

result = cveta2.fetch(
    123, output_dir="export", download_images=False,
    cache="off",
)
print(result.dataset)
rows = cveta2.fetch_task([456], output_dir="selected", project=123, download_images=False)
```

API functions never prompt. `Connection(config_path=...)` selects config;
explicit fields override inherited connection values. Cache modes are `use`,
`refresh`, `off`; `images_dir` conflicts with `download_images=False`.
Full fetch returns `PartitionResult` (`dataset`, `obsolete`, `in_progress`
DataFrames and `deleted_images` list), selected fetch returns a DataFrame.
Upload returns task ID/name/URL and image/deleted/annotation/issue counts.

By default exhausted task-level 5xx reads can skip a task and leave partial
fetch output; `CVETA2_RAISE_ON_FAILURE=true` aborts instead. Exhausted 429
aborts; reduce concurrency and inspect the error. Non-idempotent writes retry
429 but not ambiguous 5xx, to prevent duplicate annotations/tasks. Report skipped
tasks/images and failed publication separately from local CSV creation. Do not
assert complete coverage from exit status alone.

## Verification and reviewed publication

These commands were introduced in cveta2 v0.11.0 (feature 008). Reconcile against
installed-version help before execution; they are not a claim about older tags.
Local CSV verification works without optional ClearML. Preparation/publication
needs the optional `clearml` extra, its normal SDK credentials and access to
configured roots. No registry release is assumed.

```bash
cveta2 verify export/dataset.csv --profile cveta2 --output-dir inspection
cveta2 verify split.csv --profile clearml-yolo --check-images \
  --image-root /data/images --iou-threshold 0.99 --clean --output-dir cleaned
cveta2 verify export --bundle --output-dir prepared \
  --project CV/datasets --dataset-name reviewed \
  --api-url https://classes.example.org/api/classes \
  --local-root /data/images --s3-root s3://images/project \
  --empty-class 17 --accept-default-classes --accept-class-changes --non-interactive
# Separate action after inspecting the prepared directory:
cveta2 upload-dataset prepared
```

Start report-only. `--clean` explicitly accepts repairs and removal of rejected
rows; never overwrite source files. Structural failures cannot be cleaned away.
Literal `NA`/`001` values and original rejected rows/positions survive reporting.
`--output-dir` is required for --clean and --bundle; ordinary checks can report
to the console without it. `--image-root` fills absent image_path cells only;
existing relative paths use the CSV directory (DataFrame: base_dir or cwd).
`--check-images` decodes local images with EXIF-aware dimensions; URI inputs do
not trigger automatic download. `--iou-threshold` is in `(0, 1]`. Reports go to a
new directory: `report.json`, `broken.csv`, and `clean.csv` only with cleanup.

Bundle input is the complete four-table fetch export, including header-only
empty tables. Assign splits externally; require nonempty train and val, with at
least one training box in train. Validation may contain only background images.
Bundle media checks are mandatory: absolute readable supported
local images, sides at least ten pixels, training box sides and area greater
than two. No split generation or automatic end-to-end pipeline is included.

Here `--project` is an exact ClearML project name. Repeat `--local-root`,
`--s3-root` and `--empty-class` as needed. Roots are independent; verify all source
path coverage before filtering and reject symlink escapes. S3 keys may be URIs
or bucket-relative keys; a bare key must identify exactly one approved bucket.
The public HTTP(S) class API's `number` becomes stable `prod_id`; renames retain
aliases and empty policy. Unknown/empty classes quarantine whole images.
Disappeared parent IDs and ambiguous identities block preparation.

New class decisions prompt only in an interactive CLI. `--accept-default-classes`
explicitly assigns remaining new classes `empty=false`; `--accept-class-changes`
accepts additions/renames. `--empty-class ID` excludes that class. Supply missing
decisions for `--non-interactive`; never invent them. API callers receive
`MissingDecisions.decisions` instead of prompts.

Inspect `prepared/dataset.csv`, `dataset.yaml`, `tasks.yaml`, `report.json`,
`broken-images.csv`, exact four source copies under `sources/`, and
`preparation.json` and its local-only exact-byte checksum `preparation.sha256`.
Only a ready bundle is uploadable. The record binds options,
classes/roots, parent and source/payload hashes. Fix inputs/settings then prepare
a new directory; do not patch reviewed files or alter the record to bypass checks.

Upload accepts only that directory on the same machine, repeats full validation,
checks current classes, root/image access and latest exact finalized nonarchived
parent, and publishes stable bytes. A newer parent or changed evidence blocks.
Legacy CSV-only parents without metadata require choosing a new dataset name.
Only dataset.csv, dataset.yaml and tasks.yaml are uploaded, after removing
inherited child files/links; images, reports and source evidence remain local.
Success requires confirmed finalization. A failure after creation identifies the
known child; inspect it before retrying and never claim rollback or finalization.

Verify exit codes: 0 for valid or explicitly cleaned valid data, 1 for unresolved
findings/cancellation, 2 for input/config/operational failure. Upload exit codes:
0 after finalization, 1 cancellation, 2 blocked/error. CVAT `upload` is unchanged.

```python
import cveta2

report = cveta2.verify(
    "export/dataset.csv", profile="cveta2",
    options=cveta2.ValidationOptions(check_images=True),
    output_dir="inspection", clean=False,
)
print(report.is_valid, report.requires_cleanup, report.broken_row_positions)
prepared = cveta2.prepare_dataset(
    "export", output_dir="prepared",
    options=cveta2.PrepareOptions(
        project="CV/datasets", dataset_name="reviewed",
        api_url="https://classes.example.org/api/classes",
        local_roots=("/data/images",), s3_roots=("s3://images/project",),
        class_empty={17: True}, accept_default_classes=True,
        accept_class_changes=True, clean=False,
    ),
)
# After the separate review, when publication is authorized:
publication = cveta2.upload_dataset("prepared")
print(publication.dataset_id, publication.version)
```

`verify_dataframe(frame, *, profile="cveta2", options=None)` leaves caller data
unchanged; `schema_for(profile)` exposes the profile schema. `ValidationResult`
separates original validity and cleanup requirement, retains clean/broken rows,
zero-based row positions and issues. `PreparedDataset.ready` controls eligibility;
`Publication` returns dataset_id/version. Operational failures derive from
`Cveta2Error`; missing preparation choices are structured `MissingDecisions`.
