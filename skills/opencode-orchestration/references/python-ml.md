# Python/ML task contracts

Read the relevant section when planning or accepting research, experiments, or ML
engineering. Follow repository tooling and supported dependency versions. Do not
impose a new package manager, framework, logging library, or formatting policy.

## Research and experiment design

Give the investigator a precise question, current hypothesis, known evidence, and
what would disprove the hypothesis. Prefer primary papers and official code/docs;
separate cited results, deductions, and untested proposals. Check paper assumptions
against the actual data/task rather than translating a leaderboard result into a
production promise. Request concise derivations only when needed to audit a result.

Before a run, establish the baseline, intervention, metric definition and direction,
data/version and split identity, model/checkpoint, preprocessing, relevant seeds,
compute/runtime envelope, and acceptance criterion. Do not invent numerical targets
when the user's scientific intent is unresolved. Use staged experiments: inexpensive
sanity check, bounded pilot, then the justified full comparison within authorization.

Keep test data out of model/threshold selection. Check entity, temporal, and duplicate
leakage where applicable; fit preprocessing on the appropriate training partition.
Compare runs under matched conditions. A new split or changed metric may require a
new baseline, not a claimed improvement over incompatible historical scores.

Log configuration, code/data identity, artifacts, and failures in the destination's
existing experiment system. Use repeated runs or uncertainty estimates when needed
to distinguish a claimed gain from noise. A seed does not guarantee determinism
across devices, kernels, or distributed execution.

## Implementation and debugging

Give implementers concrete I/O contracts: shapes and axis meanings, dtype/device,
units, valid ranges, batching/masking semantics, and tolerance where relevant.
Select checks that challenge behavior, including empty batches, missing classes,
nonfinite values, device mismatch, gradient flow, train/eval mode, or checkpoint
round trips when implicated. Do not turn this list into mandatory tests for every edit.

For a bug, reproduce the original failure when feasible and preserve a regression
case. Distinguish data defects, math/metric defects, library behavior, and execution
environment problems. Inspect installed versions before guessing APIs. Do not repair
an upstream dependency or change a pin without authorization.

For performance changes, preserve outputs within justified tolerances and compare
against the corrected baseline. Record warmup, synchronization, batch/shape, precision,
hardware, software, memory, and repeated timings as relevant. Include preprocessing,
transfer, and postprocessing if the claim concerns end-to-end throughput. Schedule
benchmarks away from competing jobs; do not terminate a pre-existing run.

## Production and acceptance

Check changed integration contracts, configuration, serialization, error handling,
resource cleanup, and observability proportional to impact. Exercise the native path
when claiming GPU execution, model loading, distributed behavior, service integration,
or artifact publication; mocks cannot establish those outcomes.

| Claim | Minimum kind of evidence |
| --- | --- |
| Software behavior is correct | Relevant checks against the final integrated state, with edge cases |
| Native execution works | Actual run, environment/configuration identity, and resulting artifacts |
| Model quality improved | Comparable baseline, valid evaluation protocol, metric evidence and uncertainty |
| Runtime improved | Matched benchmark protocol, correctness comparison, and measured variability |
| Hypothesis is supported | Reproducible experiment or primary evidence with explicit assumptions/limits |

If resources or data are unavailable, report the exact unverified claim. Do not
replace a requested real experiment with synthetic evidence and call it complete.

## Example orchestration

Request: repair an evaluation metric and improve throughput. Both touch the same
module; one shared GPU is occupied.

Kimi establishes the metric contract and captures current changes. DeepSeek checks
the metric definition and designs a comparable benchmark read-only. Qwen owns the
metric fix and regression test. After that fix is accepted, transfer implementation
ownership for the optimization. Reserve an authorized GPU window; compare the fixed
baseline with the optimized version using the same split/configuration. Start a new
DeepSeek reviewer session for the final diff and evidence. A changed split, passing
unit tests, or a single fast run alone does not prove the requested improvement.
