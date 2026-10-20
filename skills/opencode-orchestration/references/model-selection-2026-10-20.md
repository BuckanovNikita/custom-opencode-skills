# Model selection assessment — 2026-10-20

This assessment replaces the active routing recommendation from
[2026-10-11](model-selection-2026-10-11.md). That report remains historical evidence.
No matched local evaluation establishes a best model for every task in this trio.

## Identity and evidence

| Requested model | Verified public identity | Evidence and limit |
| --- | --- | --- |
| Qwen38-27B | `Qwen/Qwen3.8-27B` | Official thinking vision-language model; distinct from Qwen3.5 and Qwen3-Coder-Next |
| DeepSeek-v4.1-Flash-0117 | `0117` not verified; official update is **DeepSeek-V4.1-Flash-1017** | Official API uses moving alias `deepseek-flash`; exact provider revision still needs confirmation |
| GLM5.3 | `zai-org/GLM-5.3`; official hosted example `glm-5.3` | Model card and hosted documentation are separate configuration contracts |

Primary sources inspected:

- [Qwen3.8-27B model card](https://huggingface.co/Qwen/Qwen3.8-27B):
  reports code, reasoning and visual-task evaluations. Its SWE-bench Pro setup
  includes corrected tasks and re-evaluated baselines; do not combine the scores
  with an unmodified split or different harness as a common ranking.
- [DeepSeek release history](https://api-docs.deepseek.com/updates/):
  October 17 update reports DeepSWE v1.1 77.0 and Terminal-Bench 4.0 42.3,
  using Mini-SWE-V2 and Claude Code respectively. Neither is a new OpenCode score.
  The older base-model card is not evidence for every property of revision 1017.
- [GLM-5.3 model card](https://huggingface.co/zai-org/GLM-5.3):
  reports engineering and long-horizon evaluations with varying harness,
  context, timeout and trial-count conditions. Those conditions prevent
  treating every published score as directly comparable.

Interpret `0117` as an unresolved requested identity, not a verified release or
automatic synonym for `1017`. Ask for clarification when exact dispatch depends on
it. An owner-confirmed internal alias remains valid only for that provider.
Public evidence here supports 1017; inactive templates deliberately require
owner-supplied provider/model IDs.

## Recommended starting profiles

| Profile | Assignment | When to use |
| --- | --- | --- |
| One model | DeepSeek primary, implementation, investigation and fresh review | Default when optional models are unavailable or add no demonstrated value |
| Two models | DeepSeek primary/general work; GLM independent review or difficult implementation | Useful second engineering perspective, with review independent of the author |
| Three models | Above, plus Qwen image-backed analysis or validated bounded work | Actual image/tool support or representative task evidence justifies the extra worker |

Use DeepSeek for synthesis and falsifiable ML/math investigation; its coding and
reasoning evidence makes it a reasonable general starting point. GLM's engineering
focus makes it an alternative for complex patches and a separate review
perspective. Qwen's explicit visual support makes it a candidate for visual
evidence gathering, not an automatic winner over DeepSeek's visual capability.
These assignments are engineering inferences. Review skill and orchestration
quality were not measured by the cited coding benchmarks.

Keep DeepSeek for ordinary implementation instead of forcing model churn. Switch
to GLM for a specific unresolved engineering problem or when matched task evidence
favors it. After GLM implements, use a fresh DeepSeek reviewer; after DeepSeek
implements, prefer GLM when available. A fresh reviewer using the author's model
still provides process independence. Qwen is optional, including for trivial tasks.

Honor explicit user choices. Model size, price, token use and diversity alone do
not establish quality. Verify actual tool support, image delivery, served context,
reasoning controls and resolved identity before dispatch. Report unavailable
controls instead of pretending prompt text configured them.

## Re-evaluate with representative tasks

Use the same repository revision, fixtures, tools, permissions, acceptance checks
and context budget across candidates. Record provider/resolved revision, adapter,
effort, quantization when relevant, failures, retries and final artifacts. Compare
correctness and completeness before speed or cost. Use repeated runs when a
selection depends on a small observed difference.

Cover regression diagnosis, a multi-file patch, evidence-based review and visual
inspection separately. Include a one-model profile, missing optional model, exact
pin mismatch and Plan-only case. The [evaluation cases](evaluation.md) specify
observable acceptance. Parser checks and Codex proxy scenarios are not live
OpenCode or target-model measurements.
