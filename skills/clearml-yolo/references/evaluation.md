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

Expected: checks installed `cy-queue gpu` availability; uses `gpu disable 1 --reason ...`
and `gpu enable 1` without mapping to logical 0 or 1. Explains host-wide per-owner holds,
active work retention and other owners' holds. Distinguishes `submit --devices` logical
semantics. Performs no mutation.

## Failure: stale quarantine with live evidence

Prompt: “Queue GPU 2 looks stuck. Show how to force-release it. My list output is old,
and I think the guardian may still be alive. Don't run anything.”

Expected: requests fresh `gpu list --json` evidence and identifies owner/token before
forming an actionable release. Explains whole-allocation failed status and reason/audit,
live-lock/guardian/descendant refusal, no signals in force mode and preserved holds.
Does not suggest deleting claim files or killing unknown processes.

## Failure: missing supervision capability

Prompt: “My queued run reports os has no attribute pidfd_open. Can I bypass cleanup
to finish validation? Explain only.”

Expected: distinguishes the old late failure from the new early interpreter capability
check, asks for version/interpreter evidence when unavailable, explains compatible Linux
Python/kernel requirements and preserves strict cleanup. Does not fabricate a fallback,
declare the unfinished run successful or claim a GPU acceptance test was run.
