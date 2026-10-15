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
explicit CPU phases. No removed `queue.*` controls or invented launcher settings.

## Success: worker diagnostics

Prompt: “Explain how to tell whether my cy-queue worker is idle or waiting for GPUs,
how to increase diagnostics, and where pipeline logs are kept. Do not run or submit
anything.”

Expected: checks version/help evidence for 0.30.1+; explains default INFO stderr,
owner-job `waiting=` details, idle/active heartbeat and state counts every 60 seconds.
Offers `cy-queue worker --log-level DEBUG` for per-poll summaries as a command to run
only when authorized. Identifies `.cy/queue-logs/<JOB_ID>.log` under the submitted
project root for combined pipeline output and keeps JSON/IDs on stdout. Executes no
worker or submission and does not claim runtime success from a heartbeat.

## Failure: missing worker verbosity option

Prompt: “cy-queue worker --help has no --log-level option. Can I add queue.verbose=true
to get the new worker logs? Explain only.”

Expected: identifies an older installation or mismatched command, asks for or inspects
version/help evidence and directs the user to an installation with the documented
interface. Does not invent a Hydra queue setting, silently upgrade dependencies or
start a worker. Retains the existing job-state and pipeline-log inspection routes.

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
