# Model selection assessment — 2026-10-11

## Decision and scope

Among the three documented models, select **DeepSeek-V4.1-Flash with thinking and
maximum supported effort** as the default for planning, orchestration, implementation,
tests, investigation, and review. Price and token consumption are not constraints.
Use the same default when authoring skills. Start in OpenCode's Plan agent; model
selection does not authorize execution.

This is a provisional engineering inference from published evidence, not a measured
three-way winner. No matched benchmark reviewed here establishes superiority for
Python/ML planning, orchestration leadership, review, or skill authoring. Task-specific
evaluation may justify a different choice. The scope does not establish that these
are the strongest models across all providers.

## Published evidence

Scores below retain their original benchmark and agent framework. Higher is better
within a comparable test; rows do not constitute a common leaderboard.

| Model | Reported results | Conditions and source |
| --- | --- | --- |
| DeepSeek-V4.1-Flash | DeepSWE v1.1: 65.5 resolved; Terminal-Bench 2.1: 85.0 pass@1 | OpenCode v1.18.15 Build with native delegation, effort 100, 1M context. [Report §§5.3.4, B.1](https://arxiv.org/html/2609.19969v1) |
| Kimi-K2.6 | SWE-Bench Verified: 80.2; Pro: 58.6; Terminal-Bench 2.0: 66.7 | Thinking; internal SWE-agent-derived framework for SWE, Terminus-2 JSON with preserved thinking for Terminal-Bench; coding averaged over ten runs. [Model card](https://huggingface.co/moonshotai/Kimi-K2.6) |
| Qwen3-Coder-Next | SWE-Bench Verified: 70.6 / 71.1 / 71.3; Terminal-Bench 2.0: 36.2 | SWE-Agent / mini-SWE / OpenHands respectively; Terminus-2 JSON for Terminal-Bench. [Report §5.1](https://arxiv.org/html/2603.00729v1) |

DeepSeek also reports 74.2 on DeepSWE and 90.6 on Terminal-Bench 2.1 using mini-SWE
and DSH Minimal respectively. Those are not its OpenCode scores. Its effort sweep
from 25 to 100 improves these results from 66.0 to 74.2 and 82.4 to 90.6, supporting
maximum effort under the stated preference. The model-card numeric scale differs
from the hosted API's `max` setting. See the [report §5.3.3](https://arxiv.org/html/2609.19969v1)
and [thinking API](https://api-docs.deepseek.com/guides/thinking_mode/).

DeepSeek's explicit OpenCode evaluation and effort results are the main reasons
for the default. They support its suitability for the intended workflow without
proving it wins every role. Kimi and Qwen also have coding-agent evidence; Qwen's
efficiency is not a selection advantage under this user's objective.

## Comparison rules and exceptions

- Do not compare Terminal-Bench 2.0 with 2.1 or SWE-Bench with DeepSWE as the same
  test. Different frameworks, tool access, sampling, and task subsets also matter.
- Do not claim a decisive advantage from small unmatched reasoning-score margins,
  parameter counts, model names, or the presence or absence of a thinking mode.
- Keep Kimi-K2.6 Thinking and Qwen3-Coder-Next as explicit alternatives when the
  user selects them or representative task results favor them. Endpoint failure
  requires a disclosed availability decision, not a silent quality downgrade.
- Same-model fresh review provides process independence; correlated errors remain
  possible. Cross-model review can add a perspective without proving greater accuracy.
- Reassess when the available models, benchmark versions, provider, or host change.
  Record actual model resolution and compare real tasks with matching tools and
  acceptance checks before claiming a local winner.

No target-model API calls or live OpenCode benchmark runs were performed for this
assessment. Published results are attributed evidence, not repository test results.
