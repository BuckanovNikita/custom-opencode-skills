# Native model preparation usage: 2026-10-29

## Scope and source

Reconciled the portable `clearml-yolo` package with released CY 0.42.0 feature
[`007-download-weights`](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.42.0/specs/007-download-weights),
against collection `origin/main` commit `13766a1`. CY release commit is
`335370d28cc2f066bc3ccec897ecb38c63efe9dc`; its annotated `v0.42.0` tag and
remote branch/tag identities were verified. Queue 0.1.3 at
`4205b9ba58f70cd69ffd9d8380ef0dd5556a3937` passed final committed-source
compatibility and was pushed. No installed skill was changed.

Changed `SKILL.md`, `references/usage.md`, and `references/evaluation.md` for the
explicit preparation command, eligible catalogue/exclusions, native caches, automatic
once-per-fingerprint policy, partial failures/interruption, retry/exit behavior,
custom model semantics, HF metadata and authentication, and no-download inspection.
The [architecture record](2026-10-29-download-weights-architecture.json) records an
inspected documentation-only no-impact decision. Runtime effects and architecture
contracts remain owned by the CY implementation.

## Checks performed

- `skill-creator/scripts/quick_validate.py skills/clearml-yolo`: passed.
- Additional YAML validation rejected duplicate keys and checked supported fields,
  folder/name agreement, nonempty bounded description, and no package symlinks: passed.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: passed; listed
  `clearml-yolo`, `cveta2`, `opencode-orchestration`, and `opencode-skill-author`.
  This was discovery only, with no skill installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'`: passed, 65 files including this report.
- CY `scripts/check_docs.py` against the three changed package Markdown files:
  passed with zero local-link/anchor/Markdown issues.
- CY `scripts/check_docs.py --all`: passed, 132 Markdown files with zero issues.
- CY `scripts/verify.py --docs` ran its checker successfully, but refused to record
  success because concurrent implementation inputs changed. This check must be rerun
  after integration stabilizes; it is not a passing integrated verification receipt.
- Architecture plan and acceptance validation: passed.

## Fresh behavioral evaluation

An independent native Codex evaluator (`portable_evaluation`, requested
gpt-6.1-sol/medium with no inherited conversation) applied the package to four
realistic prompts and compared the responses with the current CY implementation.
It had no write ownership and performed only read-only source inspection.

| Prompt | Observed response and result |
| --- | --- |
| Prepare a custom Hydra tree with `my-shape.yaml` and `pretrained=yolo11n.pt` | Correct eligible catalogue/native locations; preserved the custom architecture, pretrained/resume values and SDK settings; required separate native offline verification. Passed. |
| An automatic attempt partly failed and the next run did not retry | Explained completed-attempt fingerprints and explicit `cy-download-weights` retry, nonzero incomplete status and retained diagnostics. Did not delete receipts/caches. Passed. |
| Read-only help/configuration and queue planning; execute nothing | Presented an inspection/submission plan without executing help, downloads or submission. Distinguished worker execution from submission. Passed. |
| timm weights arrived but `config.json` failed | Refused to claim readiness; described config-only repair at the actual commit while retaining valid weights. Passed. |

No material routing or factual issue was found. The evaluator inspected the
committed baseline: it lacks the command and automatic catalogue invocation,
which validates retaining the installed-version capability guard. The parent
also ran the installed `cy-download-weights --help` from outside the checkout in
the isolated paired CY/queue environment; it exited zero and advertised only
the preparation command and help option.

This is native Codex behavioral evaluation and source comparison, not live
OpenCode target-model execution. No live OpenCode performance claim is made.

## Integrated review and release boundary

Fresh full-scope native reviewers inspected this package alongside CY and queue
implementation/evidence and found no package or documentation blocker. Their
feature-level diagnostic findings were corrected and independently closed in CY's
feature 007 review. The package's installed-version guard and native-cache/retry
guidance agree with the final implemented interfaces. Final CY release identity
and remote verification are recorded above.
Application verification is recorded separately in CY feature 007, including actual
downloads, native offline loaders and live CPU/GPU ClearML execution. Package
structural checks alone do not establish those application outcomes.

## Prepared collection patch release

Prepared collection `VERSION` 0.6.1 and a dated matching changelog entry. The
source release uses a Conventional Commit and matching annotated Git tag;
installed copies remain untouched. Current checks passed again: CLI discovery
listed all four expected skills; complete collection Markdown lint checked 65 files;
CY's local Markdown/link checker checked 64 governed collection documents with zero
issues; changed skill frontmatter validation and architecture plan/acceptance
validation passed. These structural checks are separate from behavioral evaluation
above and the final integrated review.

Final application acceptance passed 4,049 ordinary tests, static/import checks,
documentation and architecture validation, plus 56 actual prepared native assets
with network-blocked loading and real CPU/GPU CY publication/comparison checks.
The final actual-version queue suite passed 518 cases with normal resolution and
matching source/installed metadata. Native SDK logging and terminal-progress
privacy corrections have reproduced real-path regressions and fresh independent
closure. Application skips and physical-target limitations remain in CY's dated
evidence; this skill release makes no additional hardware or quality claim.
