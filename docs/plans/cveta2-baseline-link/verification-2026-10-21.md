# cveta2 baseline link evidence — 2026-10-21

Source: cveta2 `refactor/cleanup-baseline`; baseline cutover commit
`e69022727a73747bbd5723e34fa96b093f42c34c`. The isolated collection checkout starts
at `545d4001d44365540a3c9c6621d66deb9359be76` (0.4.1).
The current dataset table and contract live at `specs/baseline.md#dataset-format-and-fetch`.

The reference link is the only skill-package edit. Runtime behavior, examples and
routing are unchanged; existing evaluations require no new scenario. No live
OpenCode or target-model evaluation is claimed.

## Checks and review

- Reviewed `MAINTENANCE.md`, `RELEASE.md`, the package validation guide, the complete
  tracked diff and all three new maintenance records. The tracked changes are
  exactly the dataset-contract URL, `VERSION` and the 0.4.2 changelog entry; the
  index is empty. No command, example, option, authorization boundary, skill name,
  trigger, executable helper or resource layout changed.
- Ran a read-only Python validator through the cveta2 checkout's `uv run python`
  environment. PyYAML safe loading rejects duplicate mapping keys, demonstrated
  with a duplicate-`name` negative control. The skill's opening YAML frontmatter,
  allowed keys, name syntax/length/folder agreement and description type/length
  pass. Optional compatibility and metadata fields are absent.
- The same validator checked all three Markdown files in `skills/cveta2`: all
  three local links, including the usage-section anchor, resolve inside the
  package; no symlinks or absolute local link targets occur. Existing references
  state when to read them. No helpers were added or changed.
- The new external URL exactly names
  `specs/baseline.md#dataset-format-and-fetch`. Its `Dataset format and fetch`
  heading exists in both the current cveta2 worktree and the full cutover commit
  above. This verifies the intended destination locally; remote availability is
  conditional on publishing cveta2 first.
- Inspected the parent-run log for
  `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: all four collection
  skills, including the unchanged cveta2 name/description, were discovered.
  This was discovery with `--list`, not skill installation or model execution.
- Inspected the parent-run log for
  `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`:
  59 files checked, zero issues. Reran the same pinned Markdown linter against
  this evidence file after updating it: one file checked, zero issues.
- Confirmed `VERSION` is `0.4.2` and `CHANGELOG.md` has the matching dated
  `2026-10-21` patch entry, consistent with the collection release policy.
- Ran the shared architecture validator with
  `--record docs/plans/cveta2-baseline-link/architecture.json --phase acceptance --repo-root .`:
  exit 0. The inspected no-impact classification is appropriate: this correction
  affects no runtime modules, imports, composition or architecture check tooling.
- Ran `git diff --check`: exit 0. Independent bounded review found no issue in
  the complete task-owned diff or its architecture classification.

## Delivery boundary

New behavioral and target-model evaluations are unnecessary for this source-link
correction; existing examples, routing and evaluation cases are unchanged.
Structural checks and discovery do not establish live OpenCode/model performance.
No commit, tag, push, installation or publication was performed by this validator.
The parent owns release after the cveta2 baseline is published.

## Publication prerequisite

cveta2 main commit `b9d93f30e5feccf06b8117005d465b7ff457b8de` is now published.
Its push passed the full mutation, version-drift and live integration gates
(37 integration tests passed). Release evaluation kept cveta2 at 0.11.0.
The GitHub contents API returned `specs/baseline.md` at that exact commit,
blob `6f5cdf04e3971f2a5c42eb838a80fbf32ee70dd7`; decoding its contents confirmed
the `Dataset format and fetch` heading. The collection's new source link now
resolves to published content.

The parent rechecked the complete release diff, architecture acceptance and pinned
collection Markdown lint after this evidence update. The release is the compatible
documentation patch 0.4.2; its main commit and annotated tag are published together
using the repository's atomic-push workflow. Final remote-ref confirmation is
recorded in the cveta2 delivery evidence.
