# cveta2 usage reference

Use the installed command's help to reconcile version differences. Examples
assume supplied inputs and authorized operations; example paths are portable
placeholders. Optional deeper references are the upstream
[CLI guide](https://github.com/BuckanovNikita/cveta2/blob/main/docs/cli.md),
[configuration guide](https://github.com/BuckanovNikita/cveta2/blob/main/docs/configuration.md),
[Python API](https://github.com/BuckanovNikita/cveta2/blob/main/docs/python-api.md),
and [dataset contract](https://github.com/BuckanovNikita/cveta2/blob/main/specs/001-project-documentation/contracts/dataset-format.md).
These follow the default branch; installed-version help takes precedence for flags.

## Installation and configuration

cveta2 requires Python 3.10 or newer. Install from a trusted Git host and selected
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
then built-in preset. `--config` is available on setup/setup-clearml,
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
cveta2 fetch -p 123 -o export --no-images --no-clearml
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
`--raw`, `--no-clearml`, or `--skip-validation` option. `--completed-only` filters
task selection. Completion requires every job to be acceptance/completed;
highest task ID determines recency and deletion wins same-task ties.

`--skip-validation` on full fetch explicitly mutates CVAT: a non-ignored task
with a nonempty full job list qualifies only when every job is
annotation/completed or acceptance/completed. It promotes annotation/completed
jobs to acceptance/completed after strict cache invalidation, then refreshes
selection. Failures abort export; successful prior changes persist without
rollback. It is disabled by default and requires authorization for that change.

Full fetch may publish CSVs as a ClearML Dataset when the optional SDK is
installed, `clearml.enabled` is true, and a project mapping is configured.
`--no-clearml`, API `publish_clearml=False`, or `CVETA2_CLEARML=false` disables
publication. `setup-clearml` configures mappings. Publication warnings can occur
after local export succeeds; inspect publication evidence separately.

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
    publish_clearml=False, cache="off",
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
