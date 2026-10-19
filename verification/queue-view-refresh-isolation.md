# Queue view refresh isolation — 2026-10-19

## Scope and source

Updated `skills/clearml-yolo/references/usage.md` and
`skills/clearml-yolo/references/evaluation.md` for per-job monitoring failures.
The entrypoint, metadata, installation and invocation policy are unchanged.
Existing unrelated ClearML cache guidance was preserved exactly; task edits add
19 usage lines and 31 evaluation lines.

The inspected collection source is `main`, version `0.2.8`, at
`70d180c2287f61a89279c6f598f7d2a54401950b`. The application baseline is `master`,
version `0.34.0`, at `7dfa425b204f0687311b707e15293d3361e0f177`, with the bug fix
under active implementation. These baseline versions do not claim the changed
interface is released. Companion delivery and independent acceptance remain owned
by the application's bug workflow.

The application's
[queue guide](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/queue.md#unavailable-task-status)
and dated
[contract amendment](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/029-shared-cy-queue/contracts/queue.md#monitoring-isolation-amendment--2026-10-19)
document the same behavior. Its native evidence is under
`.specify/bugs/queue-view-refresh-isolation/` in the application checkout.

## Baseline observation and changes

Before editing, the usage reference covered archive visibility and strict worker
installation validation, but did not describe individual private refresh failures,
transient warning fields, or retained TUI rows on later queue-wide errors. This is
a static retrieval gap, not a fresh-agent behavioral failure.

The new guidance preserves readable public summaries and healthy refreshes, scopes
warnings to visible jobs, and names `warning_count` plus the `job_id`, `reference`
and `message` fields in `refresh_issues`. It separates monitoring from outcome
changes, recovery, GPU release, snapshot migration and strict execution validation.
It also separates per-job warnings from invalid shared metadata, configuration or
coordination failures. No private configuration values appear in the examples.

Added success and failure prompts exercise these distinctions. The package's
existing discovery description already covers shared queue usage, so no trigger
or entrypoint change was needed. The application README and current contract index
already link the queue guide; the authoritative documentation location is unchanged.

## Checks performed

| Check | Observed result |
| --- | --- |
| `uv run --frozen --no-sync python scripts/check_docs.py docs/queue.md specs/029-shared-cy-queue/contracts/queue.md` in the application | Passed: two Markdown files, zero issues |
| Markdownlint CLI 0.23.3 on those two application files using the collection's Markdown configuration | Passed: zero issues |
| `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'` in this collection | Passed: final repeat including this evidence file checked 41 files, zero issues |
| `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` | Passed: all four skills discovered, including `clearml-yolo`; list-only, no installation |
| Codex skill-creator `quick_validate.py` against `skills/clearml-yolo` | Passed: skill valid |
| YAML validation with duplicate-key rejection, name/folder agreement and supported metadata fields | Passed |
| Packaged Markdown link containment and target existence | Passed: all four local links resolve inside the package |
| `uv run --frozen --no-sync cy-queue list --help` | Passed; confirms `--json`, `--archived` and `--all` |
| Current core, application, runtime adapter and presentation source inspection | Confirmed diagnostic defaults/fields, visible-only filtering, strict worker refresh and per-job monitoring separation |
| Comparison against captured working files before editing | Passed: both references contain additions only; every pre-existing line remains unchanged |
| `git diff --check` in this collection and task-scoped application documentation | Passed |

No baseline Markdown or link failures were observed. No live GPU, service, worker,
or queued workload was run for this documentation task.

## Behavioral evaluation and release status

Fresh evaluation of the two new prompts is delegated to the parent workflow's
independent reviewer after integration. It is not established by the structural
checks above. No live OpenCode or DeepSeek/Kimi execution was performed; native
agent review cannot establish those host/model results.

The parent workflow owns review reconciliation, companion commits and each
repository's authorized Git-tag release. This document does not claim release,
installation, or completion of those remaining stages.

## Independent review and prompt evaluation

On 2026-10-19, fresh native reviewer `/root/final_review` (requested GPT-6.1 Sol, high
effort; runtime settings not independently exposed) returned **ship**, with no unresolved
findings. The application contract's recovery wording was corrected and rechecked:
recovery retains strict ownership/cleanup checks; admission/execution validate snapshots.

For “Success: partial queue monitoring”, the reviewer recommended the documented
all/archived listing views, described visible-only `warning_count` and `refresh_issues`,
and explained retained summaries, healthy updates and cleared warnings without Hydra
evaluation, migration, recovery, outcome changes or GPU release. For “Failure: shared
queue refresh error”, it correctly treated invalid shared metadata as queue-wide,
identified retained TUI rows as the previous successful display, and preserved task
records/reservations while diagnosing configuration/metadata. Both cases passed their
criteria without running commands or workloads. This is explicit native skill reading
and application, not automatic routing or live target-model evidence.

Patch release 0.2.9 is prepared in an isolated checkout excluding unrelated cache edits.
The no-impact architecture record is in
[the task plan](../docs/plans/queue-view-refresh-isolation/plan.md).
