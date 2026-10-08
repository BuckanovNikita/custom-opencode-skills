# Model-specific prompting

Read the section for the model receiving a task. Official guidance below was
checked on 2026-10-08; recheck it when model versions or host adapters change.
Exact-version sources largely specify runtime behavior, not a proven special
prompt formula. The example prompts are proposed engineering practices, not vendor
claims or measured superiority on Python/ML tasks.

## Shared prompt design

State the desired result first. Separate established facts, assumptions, and unknowns.
Give actionable constraints and acceptance evidence; avoid repeated motivational
instructions, theatrical personas, or requests to narrate every reasoning step.
Use one concrete example only when it resolves a real ambiguity. Ask for a concise
decision rationale or derivation when auditing it matters, not private reasoning.

Keep durable role instructions in the agent and task-specific facts in the dispatch.
Do not paste this entire reference into every worker. Allow targeted exploration
when supplied context is insufficient. Never instruct a worker to guess missing
data, suppress failed checks, or fit a conclusion to a desired metric improvement.

## Kimi-K2.6: orchestrator

**Documented behavior.** K2.6 supports thinking and instant modes. Its exact-version
API guide does not support `reasoning_effort`; do not import that knob from other
Kimi models. Thinking/tool continuity has protocol requirements. These are adapter
concerns, not instructions to print thinking into the coordinator's visible answer.
See the [K2.6 guide](https://platform.kimi.ai/docs/guide/kimi-k2-6-quickstart) and
[thinking guide](https://platform.kimi.ai/docs/guide/use-thinking-models).

**Recommended workflow prompt.** Keep the global objective, acceptance criteria,
dependencies, and decision ledger with Kimi. Make checkpoints explicit for long
tasks so progress is tied to accepted artifacts rather than long conversation.

```text
Coordinate correction of evaluation semantics and a throughput improvement.
First establish the metric contract and a comparable baseline from repository
evidence. Identify decisions blocking implementation. Assign bounded tasks only
after ownership and dependencies are clear. Keep acceptance and integration here.
Report verified outcomes, decisions needing input, and the next evidence-producing
action. A throughput gain must preserve corrected outputs and use matched inputs.
```

## DeepSeek-V4.1-Flash: investigator or independent reviewer

**Documented behavior.** The hosted `deepseek-flash` identifier is an alias; verify
the resolved version when exact identity matters. Current thinking documentation
requires retaining historical assistant `reasoning_content` when supplying tools,
including turns without tool calls. Do not apply older DeepSeek advice about
discarding reasoning history or avoiding system prompts. Let the host adapter
maintain protocol state; do not manually strip messages to save context.
See [model identity](https://api-docs.deepseek.com/en/),
[thinking mode](https://api-docs.deepseek.com/guides/thinking_mode/), and
[chat messages](https://api-docs.deepseek.com/api/create-chat-completion/).

**Recommended workflow prompt.** Ask a narrow, falsifiable question and give access
to raw evidence. Separate investigation from review into distinct sessions. A
reviewer should assess the final artifact, not defend an earlier proposed solution.

```text
Independently assess whether this evaluation change implements the stated metric
and whether the claimed improvement follows from the attached run artifacts.
Read the metric contract, final diff, and both run configurations. Check aggregation,
split comparability, leakage, and numerical edge cases where applicable. You own no
writes and may not launch experiments. Return blocking findings with locations and
evidence, missing checks, and accept / revise / blocked. Request any execution you
cannot perform from the coordinator. Do not infer success from passing unit tests.
```

## Qwen3-Coder-Next: implementation and tests

**Documented behavior.** This exact model is non-thinking only; do not send `/think`
instructions, require `<think>` blocks, or configure a reasoning budget. The model
card provides tool-use examples and sampling recommendations; those do not establish
a model-specific chain-of-thought prompting recipe. See the
[official model card](https://huggingface.co/Qwen/Qwen3-Coder-Next).

**Recommended workflow prompt.** Give a concrete interface and a bounded result,
with enough freedom to inspect code and diagnose failures. Avoid prescribing an
unverified fix. Have the worker establish behavioral evidence before claiming done.

```text
Implement the accepted metric correction in the assigned evaluator module and
regression tests. Use the repository's metric contract and existing test conventions.
Confirm shapes, masking, aggregation, and empty-input behavior from their callers.
Reproduce the reported failure where feasible, then verify the corrected behavior.
Own only the assigned paths. Do not change dependency pins or run GPU experiments.
Return changed paths, exact checks/results, decisive evidence, and unresolved issues.
```

## Optional runtime notes, separate from prompts

These are source-specific recommendations, not portable OpenCode frontmatter to
copy blindly. Leave examples free of sampling/effort overrides until the adapter's
supported settings are known. Provider setup is outside this package's scope.

| Model | Source-specific notes |
| --- | --- |
| Kimi-K2.6 | Hosted guidance uses temperature 1.0 in thinking, 0.6 in instant, and top_p 0.95; unsupported overrides may error. Do not set a generic low coding temperature. |
| DeepSeek-V4.1-Flash | Hosted thinking uses low/high/max effort levels and ignores temperature. Model-card benchmark parameters are a different interface, not API defaults. |
| Qwen3-Coder-Next | Model-card recommendation is temperature 1.0, top_p 0.95, top_k 40. Pass only settings supported by the actual adapter. |

For DeepSeek's distinction, consult the thinking guide above and the
[V4.1 model card](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash).
For Kimi's mode and sampling distinction, consult its API guides above and the
[K2.6 model card](https://huggingface.co/moonshotai/Kimi-K2.6).

Context hygiene means focused inputs, useful summaries, and fresh task sessions.
It does not mean deleting required reasoning/tool messages from an active API loop.
Do not assume advertised maximum context is the served limit, a quality target, or
permission to fill a worker with unrelated history.
