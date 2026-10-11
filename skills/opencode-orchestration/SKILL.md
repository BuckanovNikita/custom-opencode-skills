---
name: opencode-orchestration
description: Use when coordinating Python/ML research, experiments, or engineering in OpenCode, including planning, decomposition, independent verification, and recovery across agents. Targets DeepSeek-V4.1-Flash by default, with Kimi-K2.6 and Qwen3-Coder-Next as explicit alternatives.
---

# OpenCode orchestration

Own the outcome from understanding the task through integration and verification.
Optimize for verified quality; price and token consumption do not determine model
selection or effort. Keep context relevant to preserve accuracy. A small task may
need no workers. This skill does not install agents, change the running model,
grant permissions, or authorize releases or deployment.

## Establish the execution contract

Read the user's objective, repository instructions, existing changes, and relevant
implementation evidence. Establish observable acceptance criteria and ask only for
material missing intent. Follow the destination's workflow and Python conventions.
Respect planning-only and read-only modes throughout, including in worker briefs.
When starting in Plan, inspect and propose the work, dependencies, and checks;
do not implement changes, run experiments, or dispatch an implementation worker.
Write a plan artifact only if the host's planning rules allow it. Begin execution
only when the user's scope permits it and the host has left Plan mode. See
[Plan startup](references/opencode-setup.md#start-in-plan) for model selection.

Inspect available agents and live tool schemas. A role name is not proof of a model
assignment. Report missing agents/models; do not silently substitute. Continue
independent work yourself when permitted, and ask about a substitute only if needed.
Read [OpenCode setup](references/opencode-setup.md) when checking configuration or
tool compatibility; do not invent per-call model switches or context-fork controls.

## Select for quality, then route by responsibility

Use **DeepSeek-V4.1-Flash with thinking enabled and maximum supported effort** as
the default for every covered role: planning, orchestration, implementation, tests,
investigation, and review. The [dated benchmark assessment](references/model-selection-2026-10-11.md)
supports this as a provisional engineering choice among this package's three
models, not a demonstrated win on every task. Recheck relevant evidence when model
versions, providers, host adapters, or task requirements change.

Preserve explicit user model choices. Prefer comparable task-specific results over
general benchmark reputation: check exact model version, benchmark version/split,
agent framework, tools, effort, and uncertainty. Do not average incompatible scores
or infer a planning/review ranking from coding scores. Use Kimi-K2.6 or
Qwen3-Coder-Next when the user selects one or relevant evidence favors it; disclose
availability blockers instead of silently substituting. Do not downgrade a worker
for price, token savings, a simple-looking task, or its role name.

| Role | Responsibility | Delegate when |
| --- | --- | --- |
| Primary | Planning, cross-cutting decisions, integration | Keep the global contract and final acceptance here |
| Investigator | Investigation and mathematical analysis | A precise uncertainty or falsifiable claim can be checked separately |
| Implementer | Implementation and tests | Execution is authorized; interfaces, ownership, and acceptance criteria are clear |
| Reviewer | Independent assessment of the final state | The parent has integrated and verified substantive changes |

Separate roles and fresh sessions provide process independence even when they use
the same model; they do not eliminate correlated model errors. A different model
can supply an additional perspective, but diversity alone is not quality evidence.
Reassign when evidence shows a mismatch; first narrow the brief or resolve unknowns.
Read only the relevant model section in [prompting](references/model-prompting.md).

## Schedule and execute

1. Split work at verifiable boundaries. Resolve interface or scientific assumptions
   before parallel implementations depend on them. Keep trivial or tightly coupled
   work with one owner.
2. Keep a compact ledger: task, dependencies, owner, model/agent, session handle,
   owned paths/resources, status, acceptance evidence, and unresolved risk. Use an
   existing task artifact where available; do not introduce a framework.
3. Start ready independent tasks, normally no more than three workers, reduced by
   host capacity and resource contention. Never assume asynchronous execution;
   use only concurrency mechanisms actually exposed by the host.
4. Give each writable path one owner at a time. Parent edits obey this rule too.
   A worker needing another owner's file reports the dependency. Transfer ownership
   only after inspecting prior work. Separate worktrees still require integration.
5. Reserve experiment resources separately from code ownership: device, output
   directory, checkpoint, dataset cache, service, and run identifier where relevant.
   The coordinator schedules shared GPU jobs; workers must not start competing runs.
6. Continue useful independent parent work. Inspect returned artifacts and changes;
   a worker's success message is not verification. Mark tasks accepted only after
   their stated checks are met.

Workers must not delegate further, broaden scope, change shared configuration, or
take over another owner's files. Preserve pre-existing jobs and user changes. Track
and clean up only resources created by this task.

### Dispatch brief

Supply only fields relevant to the task, with concrete values rather than unresolved
placeholders. Reference source files and contracts; include essential facts inline
when the worker cannot access them.

```text
Outcome and acceptance:
Relevant facts, inputs, and source paths:
Unknowns to resolve (do not assume):
Starting state and dependencies:
Owned writable paths / read-only scope:
Permitted execution and resource allocation:
Constraints and non-goals:
Checks required and evidence location:
Return: result, changed paths, commands/results, uncertainties, blockers.
Do not delegate further. Preserve other changes. Escalate ownership conflicts.
```

## Control context without losing evidence

Keep the parent focused on decisions, contracts, integration, and acceptance. Send
bounded briefs, not transcripts or full repository dumps. Workers search first,
then read relevant slices and expand only when evidence requires it. They may obtain
necessary context; brevity must not force guesses.

Keep bulk logs, datasets, per-sample predictions, and full benchmark outputs in
authorized artifacts. Return concise conclusions with paths, run IDs, decisive
excerpts, and caveats. Do not truncate away failures or uncertainty. Open the cited
evidence when it affects acceptance; do not reload every worker transcript.

Reuse a session for the same coherent task and useful follow-up. Start a fresh one
for unrelated work or independent review; give reviewers requirements, final changes,
and evidence without the implementer's persuasive narrative. If fresh sessions are
unavailable, disclose reduced independence. Never claim a prompt erased old context.

When context becomes noisy, checkpoint accepted decisions, current artifact state,
ownership, failures, and next actions before continuing through supported host
mechanisms. This is a workflow handoff, not permission to alter API message history.
Avoid arbitrary token caps, repeated polling, and exhaustive narration.

## Recover, integrate, and finish

On failure, classify it: unclear contract, incorrect implementation, insufficient
evidence, environment/resource blocker, or model/tool mismatch. Retry only with a
changed hypothesis or new evidence. Repeated identical failures require a narrower
task, reassignment, or a concrete blocker report. Preserve useful partial artifacts;
inspect partial writes after interruption before handing ownership to anyone else.

For conflicting results, compare assumptions, inputs, revisions, and reproduction
steps. Use a discriminating check; agent votes and model reputation do not settle
correctness. Parallelize investigation, not incompatible fixes to the same file.

The parent inspects the integrated diff and runs relevant checks against the final
state. Read [Python/ML verification](references/python-ml.md) for scientific and
runtime claims. Obtain fresh independent review for substantive changes. Resolve
blocking findings, rerun affected checks, and review material corrections before
acceptance. A read-only reviewer can request parent-run checks without receiving
shell access.

Finish with the outcome, checks actually performed, supporting artifacts, and
remaining limitations. Distinguish code correctness, native execution, and measured
ML improvement. Complete required documentation and authorized repository workflow
stages; do not infer publication authority from this skill.
