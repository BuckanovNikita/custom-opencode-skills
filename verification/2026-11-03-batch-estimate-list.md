# Saved batch-estimate coverage guidance, 2026-11-03

The isolated collection source starts at main `42a201b`, version `0.7.1`.
The companion CY parent owns the implementation, final source identity, application
checks and release. The documentation checks installed `--list` support rather
than promising this interface on an older installation.

## Scope and architecture

Updated `skills/clearml-yolo/SKILL.md`, `references/usage.md` and
`references/evaluation.md` describe read-only saved coverage, existing model
selection and aliases, recorded reasons/context, totals/catalogue path and the
separation from current-run compatibility. Usable saved results cover the model
regardless of other failed contexts. Missing evidence has no invented cause;
pending snapshots retain their uncertainty. Measurement controls are rejected in
listing mode and corrupt or unreadable evidence is an error, never an empty hit.

The [architecture record](2026-11-03-batch-estimate-list-architecture.json)
records the inspected lack of runtime-module or executable-contract impact.
Original dirty source worktrees and installed skills remain untouched. This
bounded update leaves version metadata and publication to the CY parent after
review and cross-repository verification.

## Checks performed

- Documentation-only architecture record: plan validation passed before editing;
  acceptance validation passed afterward.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: discovered all four
  packages, including `clearml-yolo`, without installing skills. The CLI's generic
  detected-agent wording does not change the `--list` discovery-only invocation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'`: all collection Markdown passed; rerun after this evidence update.
- The Codex `skill-creator/scripts/quick_validate.py` accepted `skills/clearml-yolo`.
  Additional YAML validation checked duplicate keys, allowed fields/types,
  name/folder agreement, name/description limits and absence of packaged symlinks.
- CY's read-only `scripts/check_docs.py` checked the three changed package
  Markdown documents with the package as root, then this evidence file with the
  collection as root; local links and structure passed.
- `git diff --check`: passed.

The parent inspected the implemented pure coverage policy, existing usable statuses,
storage validation and early read-only CLI dispatch. Both documented listing
examples ran through the actual isolated console command with CUDA visibility
empty: exit 0 and no workspace files created. Explicit --imgsz/--refresh combinations
exited 2 without changing the workspace. CY focused checks passed 357 cases and
its generated documentation validation passed. Collection release metadata is
prepared as 0.7.2; final fresh review and publication are recorded after execution. The two new evaluation prompts are explanation-only; no live
OpenCode or named target-model execution is claimed. No GPU, training or ClearML
publication claim follows from these documentation checks.

## Fresh native usage evaluation

Initial independent review identified missing observed evaluation evidence. The
parent then dispatched two separate fresh contexts for the added prompts. Both
explicit-read explanation evaluations passed their scoped rubrics; see the
[observed prompts and outputs](2026-11-03-batch-estimate-list-evaluations.md).
Automatic routing, live OpenCode/DeepSeek execution and installation remain untested.
A scoped independent re-review is required after this evidence addition.

## Independent review accepted

Fresh native reviewer `review_companions_final` returned **ship** after inspecting
the complete tracked/untracked diff, maintenance/release policies, implemented CY
listing, command evidence and fresh explicit-read evaluation results. No functional
guidance mismatch or blocking issue remained. The missing-evidence response is a
scoped behavioral pass: it demonstrates uncertainty, preservation and mode rejection
without repeating every output column, fallback detail or exit-code expectation.
It is not full coverage of every rubric item. The new evaluation evidence is included
in final architecture scope. Version/changelog agree at 0.7.2. Collection publication
follows the actual CY release; no installed packages or host configuration changed.
