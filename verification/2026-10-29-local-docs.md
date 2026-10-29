# Local Russian CY reference skill verification — 2026-10-29

Source package: [clearml-yolo](../skills/clearml-yolo/SKILL.md).
Baseline: `44426a6b6fce058ec9ab37588483eb4bc860ca0a`, with pre-existing
uncommitted augmentation/training guidance preserved. Changes add local-reference
discovery, [usage guidance](../skills/clearml-yolo/references/local-docs.md), a usage
link and five [evaluation prompts](../skills/clearml-yolo/references/evaluation.md).
No installed copies, model configuration or runtime tools were changed.

Tool evidence: CY branch `master`, source baseline
`0037059dcf8a8ed16582f256b98036480cabd842`, installed metadata `0.41.0`, plus
uncommitted feature `008-local-docs` and its preserved feature 007 prerequisites.
The new interface and linked CY documentation contract are not in that baseline
commit. [Feature evidence](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/008-local-docs/evidence)
is authored in the tool checkout and becomes remotely available only after delivery.
CY publication is blocked until the existing 007 work is accepted and a final
proposed-version CY/queue pair is verified. All new interface guidance is therefore
explicitly marked unreleased.

## Structural checks

Collection discovery via `skills@1.7.1 add . --list` listed all four skills and the
updated description. Source-package frontmatter/schema, contained local links,
`quick_validate.py`, collection Markdownlint and scoped CY Markdown/link checks
passed. These are structure/discovery checks, not proof of model behavior.

## Fresh behavioral evaluations

Five separate native Codex agents received only the skill path, a user prompt,
read-only constraints and an instruction not to inspect the expected criteria.
They had fresh context (`fork_turns=none`), no write ownership and no further
delegation. No model/effort override was requested; actual runtime model settings
were not observable. The parent compared each response with the authored cases.

| Prompt case | Agent | Observed behavior |
| --- | --- | --- |
| Open Russian offline docs without automatic browser | `skill_eval_offline` | Selected skill from description; read local-docs; supplied `--no-browser`, `--check`, loopback URL/port/cleanup; inspected help and assets; explicitly did not claim an unexecuted full check |
| Inherited prediction size and environment CSV | `skill_eval_preview` | Separated expression from selected value; kept environment unresolved; explained overrides/download/deferred checks; no environment, dataset or model access |
| 101 jobs, external search path and target execution | `skill_eval_limits` | Retained 100-job limit and execution boundary; suggested smaller previews and named YAML; explained actual terminal-run prerequisites and typed customization |
| Missing command in older environment | `skill_eval_old` | Did not infer training failure or upgrade; explained version/environment mismatch and matching reference/help; disclosed older environment was not inspected |
| Unrelated database reference site | `skill_eval_nearmiss` | Read frontmatter only, rejected CY routing and did not invoke its commands or implement during evaluation |

All five satisfied the evaluated routing and behavioral boundaries. They used
read-only file inspection; no workload, installation or external write occurred.
These results apply to the observed native Codex evaluations. They do not establish
DeepSeek, GLM, Qwen, Claude or live OpenCode behavior. No tested response is treated
as evidence that training succeeds.

## Delivery state

The source-commit candidate contains only this task's additions on the collection
baseline; pre-existing working edits in overlapping files remain unstaged. Snapshot
Markdown/links, package validation and independent review cover that exact candidate.
The pending changelog entry describes unreleased CY guidance. This source commit
does not publish a collection version or install any skill. Collection release
still requires the clean-tree and version/tag checks in RELEASE.md; CY delivery
remains blocked as described above. The
[architecture record](2026-10-29-local-docs-architecture.json) documents the inspected
no-runtime-impact scope.
