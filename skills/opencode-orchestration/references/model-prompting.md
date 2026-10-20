# Prompting the selected models

Read the shared contract and only the relevant model/task sections. Runtime facts
below come from primary documentation; the task recipes are engineering choices
to evaluate, not vendor-certified optimal prompts or measured model rankings.
The [selection assessment](model-selection-2026-10-20.md) records identity and
comparison limits. Use the live adapter schema for settings.

## Shared task contract

Lead with the deliverable and its acceptance check. Supply the relevant repository
paths, known facts, unresolved assumptions, ownership, dependencies and execution
limits. Delimit quoted logs/data from instructions. Put the expected return shape
at the end: status, result, changed paths, observed checks, evidence and blockers.

Use one example only when a schema, edge case or output format is ambiguous.
For a structured extraction, show the schema and an unknown-value representation;
validate the returned data. For coding, supply observable behavior and relevant
interfaces, allowing the worker to diagnose the implementation. For investigation,
ask which evidence distinguishes competing explanations. For review, ask for
actionable defects or an explicit finding of none.

Ask for concise conclusions, assumptions and verification evidence. Do not request
private reasoning transcripts, embed model control tokens, or prescribe an
unverified solution. Tool calls must use actual host schemas; a proposed command
is not an executed check. Keep a compact decision ledger across long tasks.

## DeepSeek: coordination, investigation and implementation

Use an outcome-first brief with acceptance criteria and a small dependency graph.
For ambiguous work, separate the observation from the hypothesized cause and ask
for the next discriminating check. Resolve interface questions before parallel
writes. For implementation, add owned files and the regression to preserve.

Example investigation brief (paths are illustrative; replace with real inputs):

```text
Determine why macro-F1 changes when the same evaluation set is rebatched.
Read src/evaluation.py, its callers, tests/test_evaluation.py and the attached
reproduction. Read-only; no experiments or delegation.
Distinguish per-batch averaging, masking and label-order hypotheses using source
locations and a minimal numerical counterexample. Do not assume the suspected fix.
Return the supported cause, remaining uncertainty, affected contract and smallest
regression check needed. Mark proposed checks as not run.
```

For implementation, extend this brief with the accepted aggregation contract,
exclusive writable paths and required commands. For orchestration, request a
dependency/ownership ledger and acceptance decisions instead of a full transcript.

The hosted API enables thinking by default and exposes `low`, `high`, `max`.
This package selects `max` for substantive work when supported. In thinking mode,
temperature and presence/frequency penalties have no effect. When requests include
tools, the adapter must retain all required assistant `reasoning_content`,
including turns without calls. Do not strip history or fabricate reasoning fields.
Source: [DeepSeek thinking mode](https://api-docs.deepseek.com/guides/thinking_mode/).

Do not transplant R1-era rules about banning system messages or examples. Diagnose
tool-loop/protocol errors at the adapter boundary before rewriting a valid brief.

## GLM: difficult engineering and independent review

Give multi-file work a stable contract and observable milestones: reproduce,
implement within the assigned boundary, run focused checks, then report evidence.
Include accepted architecture decisions and the exact failing output when handing
over recovery work. Let feedback from checks guide the next action; repeated
unchanged retries do not advance the task.

```text
Implement the accepted batching fix across src/evaluation.py and
tests/test_evaluation.py. The metric is defined over the complete dataset and must
be invariant to batch boundaries; preserve the public result schema.
Inspect callers and existing edits. Reproduce the supplied failing case, implement
the correction and run the named regression and relevant integration checks.
You own only those two paths. Do not change dependency pins, start GPU work or
delegate. If the contract requires another file, report it before editing there.
Return complete/partial/blocked, changed paths, exact checks/results and evidence.
```

This recipe is useful for DeepSeek too. Choosing GLM for it is conditional on
availability and a concrete need for an alternative, not a requirement to hand off
all large tasks. Z.ai describes GLM-5.3 as oriented toward complex engineering and
long tasks; that supports evaluating it here, not asserting universal superiority.
Source: [GLM-5.3 overview](https://docs.z.ai/guides/llm/glm-5.3).

For local GLM-5.3 templates, documented effort levels are `low`, `high`, `max`,
defaulting to `max`. The template defaults `clear_thinking` to `false`; the card
separately recommends `true` for chat. Keep agent tool-loop settings and ordinary
chat settings distinct. Confirm hosted-provider controls separately; benchmark
sampling values are not portable OpenCode settings.
Source: [GLM-5.3 model card](https://huggingface.co/zai-org/GLM-5.3#note).

Preserved thinking is adapter-managed protocol state. Z.ai's guide requires
unchanged, complete reasoning blocks when preserving them. A reviewer must start
a fresh task, rather than inheriting an implementer's hidden history.
Source: [Z.ai thinking modes](https://docs.z.ai/guides/capabilities/thinking-mode).

## Qwen: optional visual and bounded work

Qwen3.8-27B is a thinking vision-language model; Qwen3-Coder-Next's non-thinking
instructions do not apply. Use actual images/documents together with a narrow
question and an explicit evidence table. For code/extraction tasks, supply a
bounded interface, expected format, unknown-value handling and a check the parent
can reproduce. A smaller parameter count is not evidence of task suitability.

```text
Inspect the supplied confusion-matrix image and its numeric CSV. Read-only.
For each claimed discrepancy, return image location, visible label/value,
corresponding CSV coordinate/value, and observation or uncertainty.
Check axis order, missing labels and normalization against the stated convention.
Do not infer exact values from indistinct pixels. If the image is not accessible
through your tools, report visual inspection blocked; analyze the CSV separately.
Do not claim a model-quality gain from the plot. No edits, experiments or delegation.
```

The model card recommends thinking with `xhigh` effort by default (`medium` and
`low` also exist), and retained thinking by default. Its thinking sampling recipe
uses temperature `1.0`, top_p `0.95`, top_k `20`; non-thinking uses `0.7`, `0.80`,
`20`. These are provider/template settings, not slash commands. Use sufficient
output capacity within actual server limits; do not force advertised maxima or
apply another model's sampling preset. Self-hosted `chat_template_kwargs` and
hosted Qwen controls can differ.
Source: [Qwen3.8-27B usage and practices](https://huggingface.co/Qwen/Qwen3.8-27B#api-usage).

Confirm the endpoint and OpenCode adapter both accept images; some deployments
serve text only. DeepSeek can also handle visual tasks through supported adapters.
If Qwen is absent or unvalidated, skip it. If no available path can inspect the
image, report that gap rather than treating CSV analysis as visual verification.

## Fresh review prompt for GLM or DeepSeek

Give the reviewer the final diff, requirements, contracts and raw check results,
without the author's persuasive summary. Omit any previous task/session handle.

```text
Independently review this final evaluator change against the supplied metric
contract. Inspect the final diff, affected callers and raw regression results.
Check batching invariance, empty/masked inputs, label ordering and compatibility
where affected. For improvement claims, check matched data/configuration and
baseline provenance. You own no writes; request parent-run checks if needed.
Return accept/revise/blocked, blocking defects with locations and evidence (or
none), nonblocking observations, and unverified claims. No delegation.
```

Use fresh same-model review when a second model is unavailable. Different models
do not replace checks or settle disagreements by voting. Resolve competing claims
with a reproducible counterexample or discriminating test.
