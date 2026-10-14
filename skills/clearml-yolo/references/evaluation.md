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
