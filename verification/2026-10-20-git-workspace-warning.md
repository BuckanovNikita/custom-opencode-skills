# clearml-yolo Git workspace warning guidance — 2026-10-20

## Scope and source

This update changes the portable clearml-yolo skill entrypoint, usage reference
and evaluation prompts for the advisory launch-directory warning. It covers the
nine experiment commands, help/config inspection, launch cwd versus CY_HOME,
supported working-tree forms, nonfatal Git verification failures and one check
per invocation. The companion application task is
[002-git-workspace-warning](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/002-git-workspace-warning).

The collection work started from `9bb4f98` (0.3.0) in an isolated worktree, then
was reconciled against newer `origin/main` commit `5bfa94e` (0.4.0) in another
isolated worktree. The reviewed cveta2 release is preserved; only this task's
clearml-yolo patch and new evidence were carried forward. Release metadata now
targets 0.4.1. Pre-existing edits in the shared checkout and the original task
worktree remain intact. Installed skills are unchanged. The
[architecture record](../docs/plans/2026-10-20-git-workspace-warning/architecture.json)
classifies this prose/metadata scope as no runtime impact.

## Source reconciliation

Inspected the companion application working-tree changes based on `3b3bd8f`,
including `adapters/runtime/git_workspace.py`, `entrypoints/hydra/common.py`,
the nine experiment entrypoints, diagnostics, README and the runtime-isolation
contract. The shared launcher captures `Path.cwd()` before runtime/Hydra setup and
calls the probe once inside console capture. Every documented experiment command
uses that launcher; config export, deduplication and queue administration do not.

The probe uses read-only `git -C DIRECTORY rev-parse --is-inside-work-tree`,
ignores inherited `GIT_*` overrides, applies a five-second timeout and suppresses
the warning only for successful `true` output. Missing Git, OS/timeout failures
and unconfirmed membership report an advisory through existing diagnostics.
No repository initialization or task abort is added. Git working-tree forms and
one-check placement match the usage guidance; runtime command results remain the
application's responsibility until its acceptance evidence is recorded.

## Validation and limits

Checks run in the isolated collection worktree:

- Installed architecture validator passed `plan` before the skill edit and
  `acceptance` after inspecting the prose/metadata-only diff.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` passed and discovered
  all four expected packages without installing skills.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'` passed for 55 Markdown files.
- Codex skill-creator `quick_validate.py skills/clearml-yolo` passed.
- A PyYAML loader rejecting duplicate keys checked supported metadata, field
  types, name/folder agreement and all six package-local links. Every packaged
  local target remains inside the skill; no symlink or private resource is added.
- The application `scripts/check_docs.py --root .` checker passed for the six
  changed collection Markdown documents, including local links and anchors.
- `git diff --check` passed.

After upstream reconciliation, the same discovery, metadata, package-link,
Markdown, changed-document link, architecture and whitespace checks were rerun
against the 0.4.0 base. The collection-wide Markdown check covers 57 files.

Application source reconciliation is complete. The application parent reports
321 focused tests passing, including 36 workspace cases; a separate architecture
run passed 251 tests. Ruff, mypy, Vulture and all 43 import contracts passed.
The parent subsequently reports the full routine suite passing: 3,407 passed,
30 skipped (one unsupported native-reflink case and 29 opt-in FiftyOne
database/view cases), seven physical-GPU tests deselected, 133 warnings,
731.07 seconds, exit 0. Native CPU DDP scenarios passed. This run used the
application's temporary source-override omission, an isolated PID namespace and
a private temporary queue root; existing host processes and shared queue state
were preserved. These are parent-reported application results, not collection
tests. See the companion
[application verification](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/002-git-workspace-warning/verification.md)
for the executed commands, environment distinctions and earlier failure observations.
The application publication prerequisite was satisfied before collection release,
as recorded below.

No native training, ClearML publication, live GPU operation or target OpenCode/model
execution is part of this documentation task. Application command tests belong to
the application acceptance evidence. Static review and native-agent explanation
scenarios do not establish target-host execution or automatic skill selection.

## Fresh explicit skill evaluation

The parent dispatched native Codex agent `/root/usage_evaluation` in a fresh context
with the two new prompts and only the skill entrypoint and usage reference. Expected
answers were not supplied. The parent inspected both exact responses:

1. Configuration inspection from a temporary directory with CY_HOME in Git:
   the response correctly explained that a warning alone is not command failure;
   actual exit status/output are needed. It selected actual launch cwd independently
   of CY_HOME, preserved help/config stdout, and treated repository initialization
   as a deliberate user choice rather than an automatic repair.
2. Unverified Git membership during a multirun: the response distinguished missing
   Git, failure and timeout from proof of repository absence. Bare repositories
   warn; unborn repositories, submodules and linked worktrees qualify if verification
   succeeds. The check is once per invocation rather than per job or pipeline stage.

Both explanation-only scenarios passed the maintained expectations. The evaluator
ran no application commands and changed no files. This checks explicit skill
application in a native Codex context; it does not establish automatic routing,
an OpenCode installation, alternate-model behavior or runtime command success.

## Independent acceptance

Fresh read-only reviewer `/root/final_review` returned **SHIP**, with no findings
requiring correction. It inspected the complete application diff, including
untracked additions, and all eight owned companion paths after checks. It found
the documented behavior consistent with the implementation, effect ownership and
dependency contracts; it independently passed the companion no-impact architecture
acceptance and whitespace checks and inspected the recorded package-check evidence.
The parent recorded its application architecture acceptance and the
[formal review](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/002-git-workspace-warning/review.md).

The final task-owned diff contains only the three clearml-yolo skill documents,
plan and architecture record, this evidence, VERSION and CHANGELOG. Upstream
cveta2 changes and the original shared checkout are preserved. Residual limits
are the application's bounded five-second Git probe and the stated absence of
physical-GPU, remote-publication or alternate-host execution claims. Collection
publication follows the remotely verified application release below.

## Application delivery

The parent published clearml-yolo
[v0.37.0](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.37.0)
with feature commit
[`a609fd7e8ac5d781b8f418e7dad2986dc80dc225`](https://github.com/BuckanovNikita/clearml-yolo/commit/a609fd7e8ac5d781b8f418e7dad2986dc80dc225)
and master release commit
[`018450a24c6043bba174ceeddc01c81113b3b70f`](https://github.com/BuckanovNikita/clearml-yolo/commit/018450a24c6043bba174ceeddc01c81113b3b70f).
The atomic master/tag push succeeded. The parent verified remote master, annotated
tag object `20d2c516e97b620acc9497685df8ce53b0ff2455` and the peeled target
`018450a24c6043bba174ceeddc01c81113b3b70f` with `git ls-remote`.

All feature commit hooks, including the full pytest check, passed; the checked
native release helper completed its commit/tag workflow. The parent confirmed
that release changes contain only the root project/lock version update from
0.36.0 to 0.37.0 and generated changelog, with dependency lock content unchanged.
Local source overrides were restored unstaged and are absent from both commits.
No package artifacts, GitHub Release object or deployment was created.

Collection 0.4.1 is prepared on a clean main checkout from the reviewed companion
commit `f652e7ae7010d121d08192e19424f4d0a55dd77e`, with this application-delivery
record committed before its annotated tag and atomic main/tag push.
