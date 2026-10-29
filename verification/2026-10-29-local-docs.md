# Local Russian CY reference skill verification — 2026-10-29

The original sections below record the earlier source-commit checkpoint. The
[collection 0.6.2 candidate](#collection-062-candidate) records the subsequent
reconciliation with released collection 0.6.1 and the proposed CY 0.43.0 delivery.

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

## Collection 0.6.2 candidate

Prepared a separate clean source checkout from released collection 0.6.1 commit
`1397314452c58fb115d451e6d7c1be02ac114a49`. Readback confirmed remote `main` and
annotated `v0.6.1` point to that commit. Reapplied only the eight-path documentation
candidate derived from `80ec2a037e008fafe401e39e9f1b507005cb9a0d`, resolving the
older baseline against current native weight preparation, training batch estimation,
augmentation, Git-warning guidance and release history. Existing lines in the
released usage/evaluation references and the complete augmentation reference remain
intact. The original shared checkout's HEAD, status and 74 file hashes were captured
privately and verified unchanged after preparation.

`VERSION` and the dated changelog prepare compatible patch version 0.6.2. The local
reference now requires CY 0.43.0 or later and retains the installed-command guard.
This is the proposed feature release boundary, not a claim that CY 0.43.0 has been
published. At this checkpoint the integrated CY checkout is based on
`f8eb8a3c198806c08c0a41161aa4e8034dec0273`, includes feature 008, and still reports
source metadata 0.42.0. The collection candidate must remain unpublished until CY
0.43.0 publication, final review and matching release identities are recorded.

Executed checks on this candidate:

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: passed; all four expected
  skills appeared, with the combined native-weight/batch/local-reference description.
  This was discovery only.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'`: passed, 67 Markdown files and zero issues.
- Installed `skill-creator/scripts/quick_validate.py skills/clearml-yolo`: passed.
- Strict YAML/frontmatter validation: passed the supported metadata schema,
  name/folder identity and 456-character description. A duplicate-key negative
  control was rejected. All 14 package-local links remain contained and resolvable;
  the package has no symlinks.
- CY `scripts/check_docs.py --root <collection> --all`: passed, 66 governed Markdown
  files and zero local-link/anchor issues.
- The integrated CY source's `python -m clearml_yolo.entrypoints.docs --help` and
  `--check`: both passed. Help exposes port, no-browser and check controls; the
  complete documentation check reported 467 fields, 156 configurations and eight
  tutorials, including its maintained tutorial composition cases.
- Source-origin/default/limit checks: passed for the actual integrated command
  module, automatic port zero, disabled-option defaults, 1 MiB request limit,
  32 documents and 15-second worker deadline. The maintained source and contract
  retain the 100-job sweep limit. Relevant module/contract hashes are retained
  privately with the command logs.
- Architecture plan and acceptance validation and `git diff --check`: passed.

Source-example checks explicitly selected the integrated CY package with
`PYTHONPATH` while reusing a prepared interpreter and dependencies. That interpreter's
installed CY distribution metadata was 0.41.0; assertions verified the actual
command module originated in the integrated source tree. These checks validate the
source examples, not an installed CY 0.43.0 package or normal dependency resolution.
The parent release's separate canonical, installation and paired checks own those
gates. No model, dataset, ClearML task, queue job or browser was launched here.

The five previously reviewed feature 008 evaluation cases are byte-identical to
`80ec2a0`, as is the detailed local-reference behavior after its introductory
availability wording. Their fresh native Codex results above remain the behavioral
evidence. The source reconciliation adds no preview/workload permission or changed
routing boundary; it keeps the newer released native-weight and training guidance.
No new agent or live OpenCode evaluation is claimed. Independent review of this
integrated candidate and final publication remain pending.

Private logs and preservation/behavior/source manifests are retained beneath the
candidate checkout's Git-private verification directory. No candidate commit, tag,
push or installed-skill update has been made at this checkpoint. The no-impact
[architecture record](2026-10-29-local-docs-architecture.json) covers all eight
candidate paths, including version/changelog metadata and this dated evidence.

## Integrated candidate review

Fresh native read-only agent `release_companion_review` reviewed all eight candidate
paths after parent verification and diff inspection. It checked the candidate
manifest and patch hash, preserved released guidance, implemented CY limits,
architecture record and discovery/Markdown/link/frontmatter evidence. Verdict:
**ship for the companion candidate**, with no blocking findings. No further
subagents or writes were used by that reviewer.

The parent separately verified a noneditable proposed CY 0.43.0 installation:
all 337 package files matched, installed documentation checks passed, and real
preview workers preserved supplied files and cleaned their temporary directories.
Normal proposed CY 0.43.0 / queue 0.1.4 resolution and all 518 ordinary queue tests
also passed. These checks close the earlier source-example metadata limitation;
actual CY publication and this collection's commit/tag/push remain pending.

## Published CY prerequisite and collection delivery

CY 0.43.0 is now published as annotated Git tag `v0.43.0`, object
`877480264094557b82ae1166b1ebbbecbbd27011`, resolving to release commit
`dbde23565a9938cfb7556819ab6a5beee24d5423`. Remote branch/tag readback matched.
The final committed CY 0.43.0 / queue 0.1.4 pair passed normal reinstallation,
source/installed metadata, all 518 ordinary queue tests, static/import checks,
installed help and documentation isolation. Installed `cy-docs --check` and an
explicit catalog-version assertion both passed for 0.43.0. Queue main was pushed
and verified at `4e18ec622121c228474dd454b0b46d4af902cf92`; its closing commit
changes only the already verified pair's dated evidence.

This closes the proposed-version/publication prerequisite above. Final collection
validation repeated discovery, full Markdownlint, local links and whitespace checks
successfully. Collection 0.6.2 consists only of the eight reviewed paths; the
parent's CY delivery record records its final commit/tag remote identities after
publication. The original dirty collection and installed skill copies remain
preserved. Delivery creates Git refs only, without package assets or deployment.
