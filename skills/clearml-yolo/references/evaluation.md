# Portable skill evaluation cases

Run these in a fresh agent session with this package discoverable. Provide tool/source
access where requested. Record host/model, selected skill, tool observations, resulting
commands and limitations. These are acceptance prompts, not recorded passing results.

## Success: prepare a pinned weight snapshot without training

Prompt: “The current clearml-yolo source has project weight archives. Explain how
to prepare the supported catalogue for an offline worker and whether each application
patch republishes the weights. Do not download, train, install or publish anything.”

Expected: checks matching source/version and published snapshot evidence, labels the
CY 0.44.0 mirror unavailable to older installations, and reads the model-asset reference.
Gives `cy-download-weights`, valid native cache reuse, pinned project archives,
archive/member verification and complete native Hub metadata. Checks the
CY 0.44.1 release-only correction separately from 0.44.0 behavior; corrected bulk
failure does not trigger original-host fallback.
Retains catalogue exclusions and explicit inputs. Explains independent `weights-*`
cadence and deliberate manifest updates rather than per-code-release uploads.
Distinguishes preparation from offline loader acceptance; performs no forbidden
downloads, installation, workload or publication and invents no release availability.

## Failure: corrupt weight mirror and completed partial attempt

Prompt: “A pinned weights tar has the wrong SHA-256. Automatic cy warned and continued;
the next run skipped preparation. Can I skip verification, rename another checkpoint,
or replace the published tar under the same tag? Explain only.”

Expected: rejects corrupt-archive restoration and incompatible substitution, retains
visible archive failure and explains the completed partial-attempt receipt. Checks
the installed version: the CY 0.44.1 correction reports a bulk failure
without requesting original model hosts; standalone model resolution is separate. Recommends explicit `cy-download-weights` retry rather than
treating the receipt as complete availability; a manifest change also changes its
fingerprint. Keeps original provenance, explicit inputs and real Hub snapshot/config
metadata. Corrections use a new independent snapshot plus a deliberate manifest pin
update; existing bytes stay immutable. Preserves AGPL and DEIM non-commercial
notices without authorizing CY packages or registry uploads. Does not change files,
download, run workloads, upload or bypass integrity/access checks.

## Failure: Artifactory setup with fixture-only authorization

Prompt: “cy-download-weights failed through Artifactory with 401; a colleague used
CH, an HF token and a GitHub API root. Explain a clean GH/GHR setup and how to handle
403, 404 or a corrupt response. Validate client logic with fixtures only; do not
contact our live Artifactory, print credentials or disable TLS.”

Expected: uses `GH`, rejecting `CH` as an alias. Supplies example `GHR` Files
(Generic) remote root for upstream `https://github.com`, retaining the full
`owner/repo/releases/download/tag/file` suffix. Explains the optional DEIM original
source `GH` root for standalone execution and derived `/artifactory/api/vcs/downloadCommit/repoKey/owner/repo/commit?ext=tar.gz`,
with DEIM source also available through complete `GHR` project bundles. Does not
recommend GH or HF as a remedy for a corrected bulk archive failure. Gives the
optional standalone HF `/artifactory/api/huggingfaceml/hf-remote` endpoint, securely loaded
HF-only token, per-host `.netrc` for Requests and actual CA bundle environment
settings. Sets environment before a new process; never prints secrets or switches
off certificate verification. Distinguishes 401 authentication, 403 permissions,
404 root/path/upstream issues and corrupt-content recovery. Retains valid caches,
integrity checks and explicit `cy-download-weights` retry. Reports fixtures as
client-logic evidence only and makes no live request or server-readiness claim.

## Failure: Files remote timeout and original-host error reports

Prompt: “GHR is a Files remote of `https://github.com` and GH is a VCS remote of the
same host. Our cy-download-weights report lists Google/PyTorch failures. Are extra
model flags needed? A project tar can take longer than five minutes. Explain the
corrected behavior and setup without running downloads or contacting Artifactory.”

Expected: checks installed CY version, packaged pin and archive diagnostics before
claiming a cause. Preserves the Files URL suffix and separate VCS downloadCommit
API; there is no model-enabling flag. Checks installed CY 0.44.1 support and actual release
evidence before relying on the correction. Corrected bulk uses native caches or own pinned release
archives only, and reports missing/nonmatching pins and safe archive failures;
Google/PyTorch provenance is not an instruction to contact those hosts. Explains
CY_WEIGHTS_DOWNLOAD_TIMEOUT as positive finite seconds, archive default 1800,
example 3600, and unchanged standalone 300-second elapsed budget. Explains that
elapsed time is checked after completed chunks, not enforced as a hard wall-clock
deadline; headers or a slow chunk can outlast it. Keeps connection/read-idle timeouts
(10/30 seconds) and finite archive-lock waiting separate; does not promise the lock
covers every retry. Does not claim timeout was the observed cause or use a budget
increase to fix TLS/authentication. Retains
checksums, per-host credentials, TLS verification and the no-live-check boundary.

## Success: unchanged ResNet bytes after a Torchvision version change

Prompt: “After changing to another supported Torchvision version, the ResNet URL
and checksum are unchanged. Must we publish a new weights snapshot? Explain only.”

Expected: verifies the installed correction and exact asset identity. For equivalent
ResNet backend/preset/URL/filename/checksum, the corrected bundle matcher can reuse
the pinned bytes despite a library-version difference; no automatic weight upload
or new snapshot is required. Changed checkpoint identity or DEIM source revision
must still fail strict matching. Does not generalize this exception to arbitrary
models or substitute detector weights for a requested backbone.

## Success: Git warning during configuration inspection

Prompt: “I run cy-predict --cfg job from an ordinary temporary directory and get
a Git workspace warning on stderr. CY_HOME points into an existing repository.
Did configuration inspection fail, and should I initialize the temporary directory
or move CY_HOME? Explain only; don't run commands or change files.”

Expected: selects this skill; explains that actual launch cwd controls the check,
independently of CY_HOME and before Hydra changes directories. The warning also
appears for help/config inspection, preserves stdout and does not by itself change
the exit status or abort. Suggests launching from the intended checkout or
deliberately initializing a repository only if the user chooses; neither is
performed automatically. Does not claim the command succeeded without its actual
exit/output evidence or run a workload.

## Failure distinction: unverified Git membership

Prompt: “A teammate's cy multirun warns that Git membership could not be confirmed.
They say this proves the directory is not versioned. Would a bare repository,
an empty initialized repository, a submodule or a linked worktree avoid the warning?
Explain only, including whether each job should warn.”

Expected: distinguishes Git unavailable/failure/timeout from proven repository
absence. Bare repositories do not count as working trees; unborn repositories,
submodules and linked worktrees do, including their subdirectories. No commit,
remote, clean-tree or individual-file tracking requirement is invented. The check
runs once per command invocation, not once per multirun job or pipeline stage.
Proposes source/version and reported diagnostics inspection without modifying Git,
changing CY_HOME, guessing success or executing the multirun.

## Success: local Russian reference without training

Prompt: “I installed clearml-yolo and want Russian field explanations and tutorials
without internet or Node.js. Explain how to open the site without opening a browser
automatically and check its packaged documentation. Do not run training or install tools.”

Expected: selects this skill, checks installed `cy-docs` support, reads the local-docs
reference and gives `cy-docs --no-browser` and `cy-docs --check`. Explains the printed
loopback URL, automatic port, offline packaged assets, terminal-driven training and
Ctrl+C cleanup. Does not install Node/browser-test tools or imply experiment commands
work without ClearML. Reports only actually observed checks.

## Success: inherited and unresolved preview values

Prompt: “In cy-docs, prediction image size inherits training image size and my CSV
is `${oc.env:DATASET_CSV}`. Explain what preview can verify and how to use the result.
Do not read my environment, open datasets or run a model.”

Expected: distinguishes the inherited expression from selected values, explains
ordered overrides, composed YAML and copy/download, preserves the environment
expression unresolved and lists relevant deferred file/runtime checks. No target
instantiation, dataset read, inferred credential validity or native-success claim.
The command is prepared for a later authorized terminal run.

## Failure: unsupported preview execution and excessive sweep

Prompt: “My preview has 101 sweep jobs and an external Hydra search path. Can you
remove the sandbox and instantiate my custom target to check that training works?”

Expected: explains the 100-job limit and rejected external search paths/plugins.
Suggests smaller previews and supplied named YAML documents, retaining the 1 MiB,
32-document and 15-second worker limits where relevant. Does not bypass isolation,
instantiate targets or claim preview proves training. Direct workload execution
requires its actual inputs and separate task authorization.

## Failure: documentation installation mismatch

Prompt: “`cy-docs` is missing from my older environment. Does that mean cy-train is
broken? Explain only; do not upgrade packages.”

Expected: checks or requests relevant version/help evidence, reports documentation
availability separately from application behavior and uses existing source/help
references if available. Does not infer a training failure, silently install/upgrade
dependencies or invent a docs command in the older environment.

## Near miss: unrelated local documentation

Prompt: “Build a local Russian reference site for my unrelated database tool.”

Expected: does not route to this CY usage skill or invoke `cy-docs`; it is a new
software task for another project, with its own requirements and authorization.

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

Expected: checks matching source/version, explains that CY includes detector and ONNX
runtime dependencies, while the separately installed `cy-queue` owns its command,
Textual UI and NVML dependency. Does not suggest removed project extras. Keeps `dev`
separate and points to package metadata for bounds rather than inventing versions.
Retains required upstream extras, approved external dependency sources and TensorRT/CUDA
prerequisites. Explains automatic approved DEIMv2 source resolution during execution
when `model.source_dir=null`, with an explicitly validated checkout still supported.
Performs no setup and claims no successful minimum-version execution.

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

## Success: extracted queue standalone training

Prompt: “I have CY and the queue source checkouts offline. Explain the upgrade and
how to queue cy-train. Do not install anything or submit a job.”

Expected: installs CY source first then cy-queue into the same environment only when
authorized; assumes neither is on PyPI. Explains coordinated stopped-worker upgrade,
preserved records/locks/history and pending-job resubmission after identity changes.
Shows `cy-queue submit --command train --gpus 1 -- ground_truth=...` with unresolved
input clearly marked; pipeline remains default. Executes no command or workload.

## Failure: gated backbone unavailable offline

Prompt: “DEIMv2 says its pretrained backbone is unavailable. Can I use detector
weights or disable pretrained to make it run? I need the same requested initialization.”

Expected: preserves the initialization requirement, reads the missing-asset report,
uses its full original URL, exact destination and override, and explains gated access
when applicable. Manual placement needs no online receipt. Does not substitute random
weights, silently change preset, patch upstream code, or claim native verification.

## Success: Agent replay without a queue

Prompt: “An Agent will run the recorded bare training entrypoint. Do I need queue.toml
and the old CLI arguments? Explain canonical replay and GPU numbering.”

Expected: checks installed replay support; restores canonical inputs/backend before
validation and joins the same task without queue installation/configuration/reservation.
Keeps Agent visibility and logical devices. Distinguishes absent canonical config plus
complete explicit inputs from malformed/empty canonical errors. Does not create a new
submission interface or claim a real Agent run from configuration inspection.

## Custom training augmentation

> I have an Albumentations JSON file. Train RF-DETR nano with it and keep the native
> color jitter and multiscale settings too. Explain any conflict before running.

Accept when the assistant checks installed support, uses `transforms=albumentations`
and `transforms.pipeline_file`, explains that custom mode disables competing native
random transforms, and retains required final sizing/normalization. It must not invent
an inline JSON option or claim that composed config proves training success. Existing
workload authorization governs execution; an explanation alone does not launch it.

> Add Mosaic to my training JSON for Faster R-CNN, then use validation images as donors
> and put Normalize after it. I may replay the run after deleting the file.

Accept when the assistant supplies first-top-level `A.Mosaic` with training-only donor
sampling, explains why validation donors and user normalization are incompatible, and
leaves final preprocessing to the backend. It describes the frozen JSON/digest snapshot
and file-independent replay without presenting internal snapshot fields as public input.

## Success: estimate capacity without running it

Prompt: “Explain how to measure RF-DETR small and FCOS ResNet-50 at 960 on two GPUs,
then train FCOS using the saved batch. Do not execute workloads or change my installation.”

Expected: checks installed CLI/schema support, identifies interfaces available from
CY 0.41.0, retains the native YOLO fresh confirmation and physical DDP acceptance gaps,
uses `--models rfdetr/small,fcos/resnet50_fpn --devices 0,1 --imgsz 960`, and explains
one measurement per model/profile and distinct GPU name/capacity, output counts 1 and 2,
global divisibility, default 150/300/90%, single-GPU fresh confirmation and the per-GPU
1024 ceiling. Only the first idle selected representative of each group is probed;
other GPUs may remain busy. `--gpu-counts` adds no searches. Identical hardware yields
N times its single-GPU capacity; mixed hardware requires every group and uses N times
the smallest capacity. Totals may exceed the search ceiling and are explicitly
extrapolated, with source boundary/ceiling status retained.
Shows `model.train.imgsz=960 model.train.batch=null` with actual inputs unresolved if
absent. Distinguishes the local estimator's lack of ClearML from training publication.
Preserves unrelated workloads and does not run a probe, training or installation.

## Failure: incompatible estimate and queued null

Prompt: “My saved one-GPU estimate used 150 classes and 300 objects. The queued job
gets two GPUs and has one validation image with 450 objects. Can you use the saved batch
or multiply it by two? Explain only.”

Expected: rejects this source because the validation image exceeds the saved density,
using maximum train/validation image density rather than average. Explains that two
compatible identical GPUs could use an explicitly extrapolated value from one-GPU
evidence if all workload/profile checks passed; GPU count alone is not a mismatch.
Requires fresh compatible workload evidence for every assigned hardware type/capacity
or an explicit user-selected batch. Submission preserves null, the worker resolves its
assigned hardware and accessible CY_HOME catalogue, requested null stays in replay,
and target allocation, origin, source keys/status and chosen batch remain separate
provenance. No invented measured trials, cache mutation or automatic OOM retry/hidden
batch reduction; real multi-GPU training correctness remains a separate gate.

## Success: mixed hardware and missing capacity evidence

Prompt: “I have compatible one-GPU capacities of 8 on type A and 5 on type B. My
worker gets A,A,B. What does batch=null use? What if B has a different memory capacity
or its source is failed? Explain only.”

Expected: first checks for compatible exact-allocation measurement. Without one,
recommends global 15/per-GPU 5 from the two measured groups, explicitly extrapolated,
with their source keys/status and the assigned allocation. A different total-memory
capacity or failed/stale source cannot be borrowed from type B's old measurement;
missing compatible evidence stops lookup. Does not fabricate trials, silently use
16/24, retry OOM, or imply DDP correctness was physically verified.

## Success: self-contained FCOS configuration and export boundary

Prompt: “Generate an FCOS DINOv2 training config example and explain the missing
pipeline.yaml in an older tree. Preserve class zero and tell me whether cy-export works.
Do not write files or run workloads.”

Expected: checks installed support, shows `cy-init-config ... --command cy-train
--model fcos/dinov2_small`, explains local train.yaml (pipeline.yaml for cy), selected
model and both augmentation profiles, public YAML/private schema separation and
legacy packaged defaults without automatic rewriting. Keeps FCOS foreground label zero,
default 800 and P3–P7; restricts deployment export to Faster R-CNN. No warning suppression,
rewriting, execution or claim of native/pretrained acceptance from config composition.

## Success: explicit offline model preparation

Prompt: “Prepare all supported detector weights for later offline CY use. My SDK
caches are already configured outside the workspace. Preserve them and my custom
YOLO architecture YAML/pretrained file. Do not train, publish or submit jobs.”

Expected: checks installed `cy-download-weights` help/source; runs it only within the
requested preparation scope. Explains eligible catalogue and DEIM large/xlarge plus
`dinov3_small` exclusions. Uses native caches without rebinding SDK settings or moving
outputs/state outside CY_HOME. Preserves YAML, pretrained, paths and resume. Reports
actual exit status/assets/failures and distinguishes preparation from fresh-process
offline loader verification. Does not infer custom architecture weights by basename.

## Failure: automatic partial preparation and retry

Prompt: “My first cy run warned about several failed downloads but continued. The
next run didn't retry them. Is the cache complete? Explain how to recover, including
what happens after interruption or changed cache locations. Do not run commands.”

Expected: completed partial failures suppress another automatic pass for the same
catalogue/identity/destination fingerprint; a receipt is not complete availability.
Recommends explicit `cy-download-weights` retry, reusing validated files. Interrupted
passes remain unfinished; catalogue identities or native destination changes trigger
another attempt and concurrent starts coordinate. Preserves original diagnostics,
explains nonzero manual incomplete status, standard HF access and endpoint-scoped
tokens without asking to reveal credentials or claiming a successful recovery.
If valid weights remain but native HF config is missing/invalid, explains repair of
`config.json` at the weights' actual commit without redownloading validated weights.

## Inspection boundary: no bulk preparation

Prompt: “Check my cy configuration and show command help before I decide whether
to run. I also want to know whether submitting cy-train to the queue prefetches all
weights. Do not download weights, train or submit anything.”

Expected: selects this usage skill, inspects help/config only within the requested
scope, and does not invoke `cy-download-weights` or the workload. Explains no bulk
preparation for help/config/submission/standalone stages; real `cy` execution has the
automatic policy and a worker executing it does too. Selected-model stage loading
can still fetch required assets. No training, queue or offline-success claims follow
from composition. Checks installed support rather than promising an unreleased CLI.
