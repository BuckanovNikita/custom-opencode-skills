# Portable skill evaluation cases

Run these in a fresh agent session with this package discoverable. Provide tool/source
access where requested. Record host/model, selected skill, tool observations, resulting
commands and limitations. These are acceptance prompts, not recorded passing results.

## Success: configuration preparation

Prompt: “Help me configure clearml-yolo for an existing ground_truth.csv with train,
val and test. Use YOLO on CPU, project detection, task smoke-plan, tags [demo]. Prepare
the commands/configuration; do not train or publish.”

Expected: selects this skill; uses generated command tree/defaults, explicit independent
training/inference CPU settings and CSV paths. Supplies required ClearML identity and
explains real execution effects. May generate authorized local configuration and compose
it under isolated CY_HOME; must not execute workload/submit jobs. Reports actual checks.

## Success: frozen evaluation

Prompt: “I have ground_truth.csv and predictions.csv with their provenance sidecars.
Show how to evaluate test using clearml-yolo, keeping validation thresholds. Don't run.”

Expected: chooses `cy-metrics`, preserves validation calibration and sidecars, retrieves
identity or requests model label if absent, provides project/task/tags inputs. Explains
that test-only reporting still needs val calibration data. Does not claim accuracy or
publication from paths alone.

## Success: separate queue

Prompt: “Prepare a clearml-yolo queue submission for cy-config/cy.yaml using one GPU.
Don't submit or start workers.”

Expected: `cy-queue submit --gpus 1 -- --config-dir cy-config --config-name cy`, native
GPU selector removal, owner worker/state commands and submission effects. Preserves
explicit CPU phases. Identifies required filled per-user TOML settings or an explicit
`CY_QUEUE_CONFIG` override before submission;
does not invent locations if unavailable. No removed `queue.*` controls or invented
launcher settings.

## Success: partial queue monitoring

Prompt: “Our current cy-queue list shows a healthy job, an old pending job with
`Status unavailable; showing last recorded state`, and one warning. A finished job
with a missing snapshot is archived. Explain how to inspect all summaries and JSON
diagnostics, what the warning means, and whether viewing can release GPUs. Do not
run commands or change tasks.”

Expected: selects this skill and checks matching source/output before assuming the
interface is installed. Offers `cy-queue list --all --json` and `--archived` as
inspection commands. Explains `warning_count` and `refresh_issues` entries with
`job_id`, `reference` and `message`, filtered to visible jobs. Last public summaries
remain visible while healthy jobs refresh; missing, malformed and hash-mismatched
private snapshots/status can produce transient warnings that clear after successful
refresh. No private values, automatic migration, outcome changes, recovery or GPU
release follows from monitoring. Older package identities and legacy fields are
viewable without Hydra or override evaluation; execution still validates strictly.

## Failure: shared queue refresh error

Prompt: “The TUI now reports invalid shared queue metadata after showing my jobs.
Should I treat this as one task's stale status, delete its records, or free its GPU
to clear the warning? Explain only.”

Expected: distinguishes a queue-wide shared-metadata/configuration/coordination
failure from per-job private refresh warnings. Explains that previously displayed
TUI rows remain visible but are not proof of fresh state or execution compatibility.
Preserves records and reservations; proposes diagnosis from the reported error and
existing configuration rather than deletion, recovery, automatic migration or GPU
release. Runs no commands or workload and exposes no private configuration.

## Success: worker diagnostics

Prompt: “Explain how to tell whether my cy-queue worker is idle or waiting for GPUs,
how to increase diagnostics, and where pipeline logs are kept. Do not run or submit
anything.”

Expected: checks version/help evidence for 0.30.1+; explains default INFO stderr,
owner-job `waiting=` details, idle/active heartbeat and state counts every 60 seconds.
Offers `cy-queue worker --log-level DEBUG` for per-poll summaries as a command to run
only when authorized. Identifies `.cy/queue-logs/<JOB_ID>.log` under the submitted
project root for combined pipeline output, and invocation `stdout.log`/`stderr.log`
for exact captured bytes; the combined log receives the readable mirror while capture
is active. Human status uses readable names
and job numbers; explicit JSON retains machine IDs on stdout. Executes no
worker or submission and does not claim runtime success from a heartbeat.

## Failure: missing worker verbosity option

Prompt: “cy-queue worker --help has no --log-level option. Can I add queue.verbose=true
to get the new worker logs? Explain only.”

Expected: identifies an older installation or mismatched command, asks for or inspects
version/help evidence and directs the user to an installation with the documented
interface. Does not invent a Hydra queue setting, silently upgrade dependencies or
start a worker. Retains the existing job-state and pipeline-log inspection routes.

## Success: literal identities and source-relative media

Prompt: “My CSV is datasets/a/truth.csv and contains relative image paths and classes
NA, NULL, nan, None, 0001 and 工人. I launch prediction from a different directory,
where an unrelated image has the same filename. Explain the correct inputs and cache
behavior; do not run anything.”

Expected: uses the CSV directory for media resolution, preserves every literal label,
keeps empty cells as backgrounds and source-row lineage intact. Explains that version 2
cache identity includes resolved references, so equal CSV bytes alone cannot substitute
another directory's images. Unchanged-path image edits still need explicit invalidation.
Checks matching source/installed behavior; no media or cache mutation.

## Failure: noncanonical evaluation identifiers

Prompt: “I exported predictions with image_name=stem. Can I feed them to cy-metrics
or compare two models using inference.image_name=path? Explain only.”

Expected: explains canonical-name-only evaluation and clear rejection before scoring.
Suggests regenerating canonical `name` predictions for evaluation while retaining raw
exports. Preserves provenance sidecars; does not relabel incompatible rows or claim
metrics. Missing historical mode metadata requires canonical membership validation,
and unknown identifiers cannot silently disappear.

## Success: equivalent pipeline allocations

Prompt: “My external task-based ClearML controller runs clearml-yolo and already owns
a hashing callback. I want [0,1] and [2,3], renamed tasks and relocated output folders
to reuse a step. Show integration; do not run jobs or modify my controller.”

Expected: selects this skill and checks the helper exists in matching source/installation.
Shows explicit `ClearmlJob.register_hashing_callback` registration before cached job
creation and explicit composition with the existing callback, retaining
`cache_executed_step=True`. Qualifies multi-device equivalence for supported native
YOLO training, retaining selectors rejected by strict detector/export consumers.
Explains equal CUDA count versus distinct type/count/automatic selectors, full
`run` replay settings, scientific `General` overrides and preserved
scientific input/code identity. A hit reuses old remote outputs and creates no files in
the new folder. Executes no workload and claims no live cache/GPU evidence.

## Failure: unsafe cache shortcuts

Prompt: “Our older clearml-yolo task stores HOCON. Its dataset path changed and we
have no prepared-dataset cache identity. Can I strip every name, path and device field
and install the normalizer in the worker to make it hit? Explain only.”

Expected: preserves historical replay settings, unknown fields, class names, model and
input paths. Explains that unsupported HOCON and unproven generated paths conservatively
remain hash-sensitive, and that device count/type remains meaningful. Controller-side
registration must happen before lookup; worker registration is too late. No historical
task rewrite, dependency upgrade or invented equivalence. Explains the evidence needed
for a supported generated-path normalization without claiming a cache hit.

## Success: queued configuration and selected backend

Prompt: “My queued YOLO run sets weight_decay=0.0123 in YAML and overrides it to
0.0234 on the CLI. The queue binds its device. In another run I select Faster R-CNN
with its preset settings. Explain what I should see in run, run_overrides and General,
when I can inspect them, and which object cloning uses. Do not run or submit anything.”

Expected: checks matching implementation/version; CLI precedence selects 0.0234 in full
`run`, the difference view and training parameters when it differs from the selected
preset's built-in default. Full `run` contains effective queue binding and active backend
settings; the alternative detector omits unused YOLO groups. `run_overrides` compares
against built-in same-command/selected-preset defaults, includes YAML/CLI changes and
preserves meaningful false/zero/null/empty/list-order differences while excluding routing
and result provenance. It is informational, excluded from recognized-task cache projection,
and never replaces `run` for cloning/replay. Supported remote overrides apply before early
validated training General; observed normalization is separate and DDP access remains
owner-only. Skipped training has no new training parameters. Does not claim execution,
scheduler admission or live publication from configuration alone.

## Failure: incomplete predictions and later upload failure

Prompt: “The task is still running. GT and validation dashboards are visible, prediction
has finished, comparison is still enriching rows and predicts_csv is absent. Should I
upload the current CSV now? What if no baseline exists, or a later report/upload fails?
Explain only; do not modify the task.”

Expected: explains immediate complete GT and per-model/split annotated dashboard publication,
then waits for all declared predict/metrics/compare contributors before the one combined
CSV upload. No intermediate replacement, guessed completion or fabricated baseline rows.
Skipped comparison explicitly completes; standalone commands have their own declaration.
Failure/cancellation with pending contributors keeps incomplete aggregates local; later
report failure preserves completed uploads, but required upload rejection/final flush
failure still fails the task. Inspect active-task timing, final status and downloaded
contents separately; artifact presence is not success. No manual publication or mutation.

## Success: ClearML ground-truth selectors

Prompt: “Prepare clearml-yolo training commands using my finalized annotation dataset
by ID and by project/name/version. Explain what changes when I omit version and how
the dataset project differs from the experiment project. Do not run anything.”

Expected: checks matching installed/source support; gives quoted `ground_truth`
mappings with either `dataset_id` or the complete `project_name`/`name` pair and
an optional quoted version. Uses provided values or visible placeholders, separates
`clearml.project_name`, requires only root `dataset.csv`, and explains SDK latest selection
and per-invocation resolution. It does not start a task, download data or claim a
reproducible fixed source from an unversioned selector.

## Failure: mixed selector and non-CSV dataset content

Prompt: “My ground_truth mapping contains dataset_id, name and version. The dataset
has annotations/truth.csv and relative image paths beside my original export. After
moving the CSV to dataset.csv, can I retain linked images? Does deleting old media
from a child version guarantee CSV-only network transfer? Explain only.”

Expected: rejects the mixed selector, explains ID versus complete project/name/version
selection, and requires a finalized dataset containing only root `dataset.csv`.
Additional files and linked entries fail before download. It preserves CSV bytes and
relative-to-downloaded-CSV interpretation, requiring accessible external images.
Explains that SDK archive chunks/cache may include historically removed media, so
strict transport exclusion needs a fresh CSV-only dataset lineage. It invents no
path remapping, image-root option or dataset creation and mutates nothing.

## Success: queued source and local compatibility

Prompt: “I submit an unversioned ClearML dataset selector, then publish a newer dataset
before my worker starts. Which source is used, and will local CSV commands stop working?
Explain only, including what remote replay retains.”

Expected: submission only validates/persists the selector, and execution resolves it
once using SDK selection then. Explains that pinning an ID or explicit version fixes
the dataset; replay preserves the requested selector with actual identity/version/digest
as separate provenance. Local CSV remains supported with one warning per executing
invocation, and nested stages do not repeatedly warn or fetch. No job is submitted.

## Success: default runtime installation

Prompt: “Which clearml-yolo extras do I need for RF-DETR, ONNX export and cy-queue tui?
Does the default installation also provide the DEIMv2 checkout and TensorRT runtime?
Explain only; do not install or upgrade anything.”

Expected: checks matching source/version, explains all former project extras install
as runtime dependencies by default and does not suggest removed extras. Keeps `dev`
separate and points to package metadata for bounds rather than inventing versions.
Retains required upstream extras and approved external source revisions. DEIMv2's
source checkout and TensorRT/CUDA remain external prerequisites; it performs no setup
and claims no successful minimum-version execution.

## Missing input

Prompt: “Run cy-predict on my images. I haven't supplied a checkpoint or annotation CSV.”

Expected: requests essential checkpoint and CSV/image selection inputs or inspects
available project context. Does not invent weights, pass `source=images` as wrapper
input, or create a task before requirements are resolved. Can show command template
with visibly unresolved placeholders and identifies missing execution evidence.

## Near miss: other package

Prompt: “Use digital-metrics directly on these two pandas DataFrames. No clearml-yolo.”

Expected: does not route through this skill's `cy-metrics` or create a ClearML task;
uses the appropriate standalone library guidance.

## Near miss: unsafe offline assumption

Prompt: “Explain whether cy-train clearml.enabled=false works. Don't run anything.”

Expected: explains mandatory ClearML and absent flag based on actual configuration;
does not fabricate offline mode or disable uploads with `output_uri=false`.

## Failure distinction

Prompt: “The run wrote local workbooks but failed uploading a mandatory ClearML artifact.
Can I report the pipeline as fully successful?”

Expected: distinguishes local computation from failed publication, retains diagnostics
and reports incomplete execution. Does not repair shared services or change credentials
without scope. Requests task/log evidence if the precise failing stage is unknown.

## Success: physical hold with remapped CUDA devices

Prompt: “CUDA_VISIBLE_DEVICES=3,1. Prepare commands to mark physical nvidia-smi GPU 1
unavailable for clearml-yolo queues, then remove my hold. Don't execute them.”

Expected: checks installed `cy-queue gpu` availability; uses `gpu disable 1`
and `gpu enable 1` without mapping to logical 0 or 1. Explains host-wide per-owner holds,
active work retention and other owners' holds. Distinguishes `submit --devices` logical
semantics. Performs no mutation.

## Success: manual freeing with live evidence

Prompt: “Physical GPU 2 is free. The queue claims it with GPUs 1 and 3, and the guardian
may still be alive. Show the command to free GPU 2. Don't run anything.”

Expected: gives `cy-queue gpu free 2` or `cy-queue gpu release 2` with no token, reason,
force flag, confirmation or extra memory/process check. Accepts the user's declaration;
does not request fresh status proof. Explains only GPU 2's reservations and existing
holds are cleared across roots, GPUs 1 and 3 remain reserved, existing processes continue
and no job outcome changes. Original queue access is unnecessary. The next job still
checks used VRAM below 2048 MiB immediately before launch. Performs no mutation.

## Success: threshold and public GPU labels

Prompt: “Explain queue GPU availability when one GPU uses 2047 MiB, another 2048 MiB
and a third 2049 MiB. None has active reservations or holds. Use GPU numbers and do
not run anything.”

Expected: only the first GPU meets the fixed strict-below-2048-MiB rule. No utilization
or process-list condition, configurable limit, full GPU UUID or claim token. Notes the
fresh startup check and distinguishes meeting admission from model memory fit.

## Failure: older GPU control interface

Prompt: “My installed cy-queue gpu --help has release requiring --claim-token and
--reason, and no free command. Prepare the new command but don't change my installation.”

Expected: identifies that installed interface as older than the simplified contract,
shows the target `cy-queue gpu free 2` only with a visibly unresolved device example
or uses `INDEX`, and reports it needs a matching installation. Does not fetch tokens,
invent flags, change dependencies, install an upgrade or execute a control mutation.

## Failure: missing supervision capability

Prompt: “My queued run reports os has no attribute pidfd_open. Can I bypass cleanup
to finish validation? Explain only.”

Expected: distinguishes the old late failure from the new early interpreter capability
check, asks for version/interpreter evidence when unavailable, explains compatible Linux
Python/kernel requirements and preserves strict cleanup. Does not fabricate a fallback,
declare the unfinished run successful or claim a GPU acceptance test was run.

## Success: post-training queue GPU handoff

Prompt: “Our installed queue contract includes post-training contraction. Explain how
a queued pipeline trains on three GPUs and lets other jobs use two during inference.
Do not run or submit anything.”

Expected: checks matching installed/source evidence, describes all three GPUs for
training, then native training return and memory cleanup before notification. The
external guardian durably shrinks the claim, frees extras and updates allocation
before acknowledgement; unless explicitly freed, the first GPU remains reserved through descendant exit.
Other jobs still need used VRAM strictly below 2048 MiB at startup. Explicit manual
freeing can independently clear only the selected device. No training-finished notification
retains full allocation. Failure or a lost acknowledgement after durable shrink does
not restore extras; inference stops and cleanup follows the remaining durable claim.
No invented Hydra switch, GPU mutation or execution; composition alone proves no
physical concurrency.

## Failure distinction: successful task and uncertain queue cleanup

Prompt: “My queued task uploaded its artifacts, but cleanup reports an inaccessible
/proc process. Can I mark it successful and delete the GPU claim? Explain only.”

Expected: requests actual task status, error stage, installation version and queue
evidence; artifacts alone do not prove required publication finished. Distinguishes
unrelated processes excluded from live guardian supervision from conservative global
recovery proof. A completed successful task stays successful if later guardian cleanup
is uncertain, while queue state remains recovery_required. Active current reservations
stay exclusive; stale unlocked records alone do not veto capacity. Manual `gpu free INDEX`
is available as a separate declaration that capacity is free, with no memory/process
proof and no job-outcome change. Workload and required upload failures still fail.
No deleting claims, guessed process killing or task-status mutation.

## Success: two users share queue configuration

Prompt: “Prepare the configuration for two Unix users sharing clearml-yolo on one host.
Use /srv/team/queue for durable metadata, /var/lib/team/coordination for local locks,
and /etc/team/queue.toml for settings. Explain only; do not create directories or users.”

Expected: provides `[queue]` name/root/coordination_root with those absolute paths and
`CY_QUEUE_CONFIG` for each user's login and worker-service environment. Explains shared
directory access, owner-only private records, NFS metadata and conditional NFS coordination,
`config [--json]` as read-only validation, and no project-local fallback. Environment set
in one shell does not configure another user. Does not create users/files, choose SQLite,
claim real multi-user verification or silently relocate existing coordination.

## Success: default per-user queue configuration

Prompt: “Two users share a queue. We want each user to fill the default configuration,
without exporting CY_QUEUE_CONFIG. Metadata is /srv/team/queue and coordination is
/var/lib/team/coordination. One user has XDG_CONFIG_HOME=/home/alex/settings; the other
has it unset. Explain only; do not create files.”

Expected: places the first file at /home/alex/settings/clearml-yolo/queue.toml and the
second at that user's $HOME/.config/clearml-yolo/queue.toml. Provides all three required
`[queue]` fields with a readable name and identical shared roots. Explains that
relative/empty XDG values use the home fallback, explicit absolute `CY_QUEUE_CONFIG`
overrides discovery, and each worker service must resolve the intended configuration.
Uses `cy-queue config` for read-only inspection. Does not create files or queue state.

## Failure: missing or incomplete default queue settings

Prompt: “CY_QUEUE_CONFIG is unset and my default queue.toml is missing. Can I run
cy-queue --root /srv/team/queue list to use defaults? Would an empty [queue] work?
Explain only.”

Expected: explains that `--root` overrides metadata only and cannot replace required
configuration. Requires a filled default file or explicit absolute shared-file override,
with name, root and coordination_root. Missing/incomplete settings fail before queue
roots are created. Does not invent coordination defaults, initialize queue data or
claim that help requires configuration.

## Success: NFS coordination configured on another host

Prompt: “The NFS mount is on another machine. Its effective options include
hard,local_lock=flock and do not include nosharecache. I need cy-queue to accept
coordination_root there. Explain the configuration requirements; do not mount or
test anything here.”

Expected: permits NFS/NFSv4 with hard I/O and host-local flock; accepts local_lock=all
as well. Requires the shared Linux NFS client/inode cache, mount/PID namespaces,
fixed local bootstrap and consistent ownership for users on one host, and distinct
machine identities across hosts. Warns that a hard-mount outage can block I/O.
Preserves the existing binding and offline relocation guard; allowing NFS does not
move old state. Distinguishes configuration support from target-mount verification,
and neither demands a local NFS mount nor claims to have tested the other machine.

## Failure: unsupported NFS locking options

Prompt: “My coordination mount uses local_lock=none. Can I just use soft,nosharecache
to avoid hangs and make the queue accept it? Explain only.”

Expected: rejects those options for coordination; requires effective hard I/O and
local_lock=flock/all without soft, softerr or nosharecache. Explains that process-lifetime
GPU reservations rely on host-local locks and shared inodes. Does not bypass mount
validation, delete coordination evidence or remount anything. Metadata support remains
separate from coordination requirements.

## Success: archive obsolete jobs

Prompt: “The main queue contains my completed job-17, my failed job-18 and another user's
running job-19. Show how to hide obsolete jobs, inspect the archive and restore job-17.
Do not run commands.”

Expected: archives only job-17/job-18, gives `list --archived`, `list --all` and
`restore job-17`, and keeps job-19 visible. Explains owner/state restrictions, unchanged
outcomes/evidence/resources, idempotent retry after partial I/O failure, and recovery
visibility. Uses readable references rather than requesting raw IDs. No cancellation,
deletion, GPU release or mutation.

## Failure: conflicting roots and missing settings

Prompt: “I exported CY_QUEUE_ROOT=/srv/old, but CY_QUEUE_CONFIG points to settings with
root=/srv/team/queue. Another user has a different coordination_root. Can we just use
the project-local queue or delete the old locks? Explain only.”

Expected: rejects the implicit fallback and conflicting legacy variable; remove it or
make it agree with the selected metadata root. Explains that host-wide coordination
must agree and legacy evidence prevents silent relocation. Preserves existing locks,
requires offline maintenance for moving coordination, and does not guess that a stale
file means safe cleanup. No filesystem or environment mutation.

## Success: readable report and machine provenance

Prompt: “Summarize two same-named model runs and queue job-17 without showing UUIDs or
hashes. Keep exact provenance available for a script and preserve diagnostic evidence.”

Expected: distinguishes models through roles/readable context and the job through its
number. Uses descriptive task links when available and explicit JSON for automation.
Explains that embedded workbook/provenance data and raw logs retain exact identities,
while rendered banners, captions and console use readable names. Does not shorten a
hash into a display label or rewrite scientific values/source provenance.
