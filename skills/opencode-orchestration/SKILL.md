---
name: opencode-orchestration
description: Use when coordinating Python/ML research, experiments, or engineering in OpenCode across agents, including task decomposition, model routing, independent verification, and recovery. Covers DeepSeek-V4.1-Flash, GLM-5.3, and optional Qwen3.8-27B.
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

Use **DeepSeek-V4.1-Flash** as the primary and general implementation default.
Add **GLM-5.3** for independent review or a difficult engineering task needing a
different approach. **Qwen3.8-27B** is optional for image-backed investigation and
bounded work supported by task evidence. One model can cover the whole workflow;
using all three is not an acceptance requirement.

These are provisional routing choices, not measured winners for each role. Read
the [selection assessment](references/model-selection-2026-10-20.md) when choosing
or changing a profile. Verify the exact provider revision: the requested DeepSeek
`0117` label is unverified; the documented release is `1017`. Never silently map
one to the other or treat a moving alias as an immutable pin.

| Task | Starting choice | Prompt emphasis and routing condition |
| --- | --- | --- |
| Planning, integration, mathematical/ML investigation | DeepSeek | Objective, evidence, competing hypotheses, dependency decisions and acceptance |
| Implementation and tests | DeepSeek | Bounded interface, reproduced failure, owned paths and executable checks |
| Difficult multi-file engineering or recovery | GLM, optional | Accepted interfaces, milestones and feedback from failing checks; switch for a concrete unresolved problem |
| Final independent review | GLM after DeepSeek work; DeepSeek after GLM work | Fresh session, final diff and raw evidence; ask for defects and missing checks, not agreement |
| Plots, screenshots, documents or bounded extraction | Qwen, optional | Actual accessible inputs, precise observations, output contract and cross-checks |

Use thinking and the highest verified supported effort for substantive reasoning:
DeepSeek `max`, GLM `max`, Qwen `xhigh` where their adapters support those controls.
Preserve explicit user selections. Provider settings belong to configuration,
not prompt incantations. If a preferred optional model is absent, disclose that
and use a capable available model or keep the task with the parent. An explicit
model pin or required capability must be satisfied before that dependent dispatch.
For images, verify both model and adapter image support; numerical checks alone
do not establish visual inspection.

Separate roles and fresh sessions provide process independence even when they use
the same model; they do not eliminate correlated model errors. A different model
can supply an additional perspective, but diversity alone is not quality evidence.
Before reassignment, resolve missing context and narrow the failure hypothesis.
Prefer matched task results over reputation, size or cost. Read the selected model
and task section in [prompting](references/model-prompting.md) before dispatch.

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
Agent/model and verified capabilities:
Owned writable paths / read-only scope:
Permitted execution and resource allocation:
Constraints and non-goals:
Checks required and evidence location:
Return: status (complete / partial / blocked), result, changed paths,
checks actually run and results, evidence locations, uncertainties and blockers.
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
