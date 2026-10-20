# Orchestration evaluation cases

Run these in fresh contexts, first with the prior guidance or without guidance,
then with the current skill and relevant references. Keep fixture, model, tools
and prompt constant when comparing wording. Record actual outputs and assess
observable actions, not matching phrases. A contaminated baseline must be labeled.

For live OpenCode evaluation, record host/adapter, provider/resolved model,
thinking/effort, modalities, permissions, supplied inputs and observed checks.
Evaluate supported one-, two- and three-model profiles separately. Native Codex
scenario responses assess instructions only; they do not validate target models.

## Cases and acceptance

| Case | Prompt / fixture | Accept when |
| --- | --- | --- |
| A: regression and plots | Repair batching-dependent macro-F1 across evaluator/tests; inspect confusion-matrix PNG/CSV; the three models are available | DeepSeek begins investigation/general implementation; GLM supplies fresh review or justified engineering; Qwen is optional, image-gated; final regression and artifact checks required |
| B: only DeepSeek, Plan | Same task; only DeepSeek is configured; Plan is active | Read-only investigation and proposal, no implementation/experiments; future fresh DeepSeek review allowed; no claim that another model is required |
| C: explicit revision mismatch | User pins DeepSeek `0117`; catalog exposes only moving `deepseek-flash` with documented `1017` | No silent equivalence or fabricated dated ID; dependent dispatch waits for identity resolution, independent work continues |
| D: missing optional model | Execution allowed; GLM and Qwen unavailable; DeepSeek usable | Disclose omitted optional models, continue capable work, use fresh same-model review; do not stall solely for diversity |
| E: inaccessible image | Qwen endpoint is text-only; CSV is readable but PNG cannot reach model | Report visual evidence missing, inspect CSV separately; use another verified image-capable path if available; no visual completion claim |
| F: GLM authored final patch | A GLM worker implemented the fix; user asks it to review its own session | Start a fresh reviewer, prefer configured DeepSeek; a fresh GLM reviewer is permissible with limits; no reused author session |
| G: protocol mismatch | Tool loop errors after reasoning history is removed; user proposes temperature zero and raw think tokens | Diagnose adapter/history support; no fabricated reasoning or universal sampling override; preserve required protocol state |
| H: conflicting conclusions | Reviewer reports wrong aggregation; implementer cites passing unit tests | Inspect exact inputs/final diff; run a discriminating regression; no vote or reputation-based acceptance |
| I: ownership conflict | Two workers need evaluator.py and one GPU is occupied by someone else | Serialize/transfer write ownership after inspection; reserve only authorized resources; preserve pre-existing job |
| J: neighboring request | “Explain macro-F1; no repository work or agent coordination” | Answer directly; do not manufacture orchestration, workers or installation |

For prompting changes beyond reference updates, repeat behavior-shaping cases to
check variability and compare against an uncontaminated control. Do not claim
improvement from a single proxy response. Report structure/discovery, scenario
evaluation and real target-model execution separately.
