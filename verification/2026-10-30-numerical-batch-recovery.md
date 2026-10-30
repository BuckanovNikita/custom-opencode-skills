# Numerical batch recovery guidance, 2026-10-30

This documentation-only change accompanies the CY
[numerical-search bug](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/bugs/yolo-numerical-search/assessment.md).
The isolated candidate is based on latest fetched collection main
`80c930cade4d766bc7db0d77024fcd3a48d3da77`, version `0.6.5`, and CY master
`dccb0a2f6b47fc6318bda2111f8ba1dda9f542c7`, version `0.44.2`, plus the parent-owned
numerical recovery implementation. Collection patch `0.6.6` is prepared; source
commits, released CY version and final review must be reconciled by the parent
before delivery. Guidance checks installed/source support rather than claiming an
unreleased implementation is already available in older packages.

## Scope and decisions

Changed source paths are `skills/clearml-yolo/SKILL.md`,
`skills/clearml-yolo/references/usage.md`,
`skills/clearml-yolo/references/evaluation.md`, VERSION, CHANGELOG.md and this
verification evidence, with a
[no-impact architecture record](2026-10-30-numerical-batch-recovery-architecture.json).
No runtime modules, executable helpers, dependencies, installed skill copies or
original working trees changed. The parent owns CY runtime architecture and acceptance.
The collection retains its existing workflow and packaging.

The guidance preserves the distinction between numerical and memory limits,
requires one complete independent repeat after discovery-time numerical failure,
retains failed trials and diagnostics, and rejects all failed confirmations.
It covers exact/extrapolated limitation precedence, training provenance, schema-1
reads/schema-2 saves, older-reader migration, read-only preservation, fingerprint
incompatibility and unusable pending snapshots. The old-version example uses the
reported workload and lower search ceiling without claiming general GPU capacity
or resolution of the underlying native numerical cause. Evaluation cases cover
both explanation-only recovery and failed/active snapshot safety.

## Executed checks

- Shared architecture validator accepted the documentation-only record in both plan
  and acceptance phases.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` found all four collection
  skills including clearml-yolo; this was discovery only, without installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'` passed for all 71 collection Markdown files with zero issues.
- Codex skill-creator `quick_validate.py` accepted clearml-yolo metadata. Additional
  YAML validation rejected duplicate keys and checked allowed fields, field types,
  name syntax, name/folder agreement and lengths.
- CY's Markdown/local-link checker accepted SKILL.md and both changed references
  with the distributed skill folder as root: three documents, zero issues. Required
  local links remained within the portable package.
- The four changed CY documents passed CY's Markdown/local-link checker with zero
  issues. The bounded old-version retry was parsed through current CY `parser()`
  and `_options()`, validating model selector, 960 resolution, 100 classes,
  300 objects/image and ceiling 24 without launching a workload.
- Current `cy-batch-estimate --help` succeeded and exposes all documented flags.
  Recovery, catalogue and provenance statements were compared against current CY
  core search/schema/recommendation policy, application progress snapshots and
  training provenance adapter. Parsing/help is not native GPU evidence.

- CY's Markdown/local-link checker accepted this evidence and CHANGELOG.md with zero
  issues. `git diff --check` passed. VERSION `0.6.6` matches the dated changelog.
  Final review and release provenance remain parent-owned acceptance work.

## Remaining parent acceptance

Fresh independent review must inspect final integration and execute the two appended
explanation-only evaluation cases in a fresh context. No OpenCode executable was
available in this environment, and no live target-model evaluation was performed.
Native A40 failure reproduction, native numerical cause remediation, GPU training
capacity, physical distributed correctness and publication outcomes are not
established by these documentation checks. The parent owns native and paired CY/queue
checks, final source/version reconciliation and Git-only releases. No commits,
tags, pushes or installation were performed by this bounded documentation task.

## Parent independent review

On 2026-10-30, fresh native read-only agent `numerical_final_review` reviewed the
final CY and collection changes after parent checks and returned **ship**, with no
findings. It independently reran all 26 CY numerical recovery tests. Both appended
explanation-only evaluation prompts passed: failed old data remains unusable, bounded
retry requires fresh confirmation, recovery preserves numerical limitations and all
native checks, schema migration preserves history, and every failed confirmation
or initial numerical failure remains unusable. Active pending snapshots are not
proof of interruption. Neither prompt's commands were executed.

This is fresh native-agent explanation evidence, not a live OpenCode/target-model
benchmark. The parent verified CY's full canonical suite (4508 passed, 30 skipped,
81 deselected) and proposed CY0.44.3/queue0.1.5 compatibility (518 queue tests passed,
7 deselected). Target A40 numerical cause and supervised GPU recovery remain
unconfirmed; the existing telemetry guard is unchanged. Final release provenance
is recorded after the CY release hooks.

## Release source reconciliation

CY's native release hook produced version **0.44.3**, source release commit
`02e2d50ad9f3dd6ea619f69adfe5ffc45c32f361`, from contributor commit
`0cf39a0`. Its annotated Git tag is `v0.44.3`; the version commit changes only
CHANGELOG.md and root package versions. The actual source is reinstalled normally
with queue 0.1.5 before pushing, as required by CY's paired release gate. Collection
VERSION and changelog agree on **0.6.6**. No packages, assets or GitHub Release
objects are part of either code/documentation release.

Final actual-source pairing passed before push: normal editable installation,
dependency consistency, source/installed metadata and CLI help agreed on CY
**0.44.3** and latest queue **0.1.5** (`397753c123f379ade50f06a2babf5e4b1476b9d2`).
The repeated full queue suite passed **518 tests**, with 7 deselected and 19 warnings.
Parent review of the complete diffs and final Markdown/local-link/architecture checks
passed; original pending version/changelog edits remain outside this isolated release.
