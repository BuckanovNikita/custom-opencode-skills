# Verify the collection from zero

## Why

On 2026-11-07 the user merged all started, unfinished and non-cancelled work into
each repository's default branch without verification. Earlier partial verification
evidence (dated records, open checklists and per-task acceptance claims) is being
dropped as proof of the current state. Everything reachable from `main` must be
verified again from a clean checkout before the next release.

For this collection the merge pass found nothing to merge: the preserved branch
`preserve/portfolio-20261104/portable-guidance` (`1e3911d`) holds only snapshot
bookkeeping, superseded "unreleased" guidance and a historical v0.6.6 publication
paragraph whose facts are already recorded by tag `v0.6.6` and
`verification/2026-10-30-batch-search-speedup.md`. Its local ref is kept.

## Range

`3c5004528151fb11f77267ff80962e2e818aac1e..11b686662c748adbf7876840af4c5dfdb965c01b`
(root commit through `main` after the merge pass; `VERSION` 0.7.5, last tag
`v0.7.5`), plus the commit that adds this plan.

## Checklist

Run from a fresh clone of `git@github.com:BuckanovNikita/corp-skills.git` at `main`.
Record observed results in a new dated verification record, not in this file.

### Setup

- [ ] Clone fresh, check out `main`, confirm `git status` is clean and `HEAD`
      matches `origin/main`.
- [ ] Confirm Node.js/npm and Git are available; note their versions.
- [ ] Confirm `VERSION`, the top `CHANGELOG.md` section and the latest tag agree, and
      list any `Unreleased` entries.

### Package and document checks

- [ ] `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` lists every package
      under `skills/` (clearml-yolo, cveta2, opencode-orchestration,
      opencode-skill-author) without installing.
- [ ] `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`
      reports zero issues.
- [ ] `git diff --check "$(git hash-object -t tree /dev/null)" HEAD` reports no
      whitespace errors across the whole tree.
- [ ] Validate each `SKILL.md` frontmatter: unique YAML keys, name equals folder,
      description present, per
      `skills/opencode-skill-author/references/validation.md`.
- [ ] Check every packaged local link and bundled resource resolves inside its package.
- [ ] Validate inactive agent examples under
      `skills/opencode-orchestration/examples/` separately (not skill frontmatter);
      `opencode.plan.json` parses as JSON with explicit placeholders.
- [ ] Parse every `architecture.json` under `docs/plans/` and `verification/` and
      confirm each records documentation-only, inspected no-impact scope.
- [ ] Check `CLAUDE.md` is a regular file importing `@AGENTS.md`, README.md is
      Russian and other docs are English.
- [ ] Scan for secrets, private endpoints, local checkout paths and machine capacity
      inside `skills/`.

### Tool-contract reconciliation

- [ ] clearml-yolo: compare `skills/clearml-yolo/` (SKILL, usage, augmentation,
      evaluation, local-docs) against the current released CY source and installed
      help for `cy`, `cy-docs`, `cy-download-weights`, `cy-batch-estimate`, queue
      commands, force/reader-upgrade behavior and DINOv3/HF guidance (CY 0.46.0).
- [ ] Confirm availability labels: nothing marked unreleased that is released, and
      nothing presented as released that is not.
- [ ] cveta2: compare `skills/cveta2/` against the current cveta2 release CLI help,
      dataset contract and managed-dataset/fetch-diagnostics behavior.
- [ ] opencode-orchestration and opencode-skill-author: confirm the model guidance
      and setup references still match the current host adapter and provider facts.

### Behavioral evaluation

- [ ] Run each package's `references/evaluation.md` prompts in fresh contexts,
      separating routing (names and descriptions only) from explicit package reads.
- [ ] Record host, model, effort, prompt and observed output for every case,
      including failure and missing-input cases.
- [ ] If a live OpenCode/DeepSeek V4.1 Flash runtime is available, run the target
      evaluation and record it; otherwise state it is untested.
- [ ] Installed-copy acceptance stays out of scope unless an installation is
      requested; record it as untested.

### Architecture and record acceptance

- [ ] Confirm no executable helpers, runtime modules or dependencies exist in the
      packages (documentation-only collection).
- [ ] Reconcile dated verification records with the current skills; label
      superseded guidance rather than rewriting history.
- [ ] Obtain fresh independent review of the complete `root..HEAD` content after
      the checks above.

### Release and pairing

- [ ] Pair the collection with the companion releases it documents (clearml-yolo,
      cveta2, cy-queue) and record their versions and commits.
- [ ] If changes are needed, prepare a patch release per `RELEASE.md` (VERSION,
      dated changelog, annotated tag, atomic push); never create GitHub Releases.
- [ ] Verify the annotated tag and remote refs match local refs.

## Supersedes

These ledgers and records no longer count as acceptance evidence for the current
state; keep them as history:

- `docs/plans/source-guidance-20261104/` and its `verification-2026-11-04.md`.
- `docs/plans/corp-conflict-reconciliation-20261102/` (open items).
- `docs/plans/claude-codex-organization-20261102/` (open item).
- `docs/plans/cleanup-baseline/verification.md`.
- `verification/clearml-yolo-046-20261107/`, `verification/dinov3-selected-20261105/`
  (open items) and `verification/pipeline-outcomes-20261105/`.
- All earlier dated records under `verification/` and `docs/plans/*/verification-*.md`.
- The snapshot ledger `docs/plans/unfinished-snapshot-20261104/` on the preserved
  branch `preserve/portfolio-20261104/portable-guidance` (not merged).
