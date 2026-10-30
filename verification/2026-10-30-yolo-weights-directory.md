# Persistent YOLO weights directory guidance, 2026-10-30

This is the documentation-only companion of the clearml-yolo
[yolo-weights-directory bug](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/bugs/yolo-weights-directory/assessment.md).
The release candidate is based on collection commit
`c985e12`, version `0.6.4`, and companion CY source `3636404`, version
`0.44.1`, plus the isolated persistent-directory correction. The companion CY release is `0.44.2`; collection patch is `0.6.5`. CY
commit `dccb0a2f6b47fc6318bda2111f8ba1dda9f542c7` and annotated tag `v0.44.2`
were pushed after canonical, independent review and final paired verification.
Examples retain the installed/source-version guard.

The task changes `skills/clearml-yolo/references/usage.md`,
`skills/clearml-yolo/references/evaluation.md`, release VERSION/changelog, this
evidence and its
[no-impact architecture record](2026-10-30-yolo-weights-directory-architecture.json).
The initial working-copy edits were snapshotted and preserved. Only the task delta
was transferred into a fresh clone of latest main; all unrelated pending changes
were excluded. The latest collection already incorporates corrected release-only
bulk preparation, so its current guidance was preserved. No runtime
module, executable helper, dependency or discovery entrypoint
changes are owned by this task. Documentation additions cover absolute persisted
YOLO cache selection, changed worker directories, idempotence, preservation,
locking/readback and migration/failure recovery. Two appended evaluation cases
cover explanation-only migration and failed settings persistence.

## Executed checks

- CY's Markdown/local-link checker passed for the eight changed CY documents.
- Skills CLI `skills@1.7.1 add . --list` found the collection's four skills,
  including clearml-yolo; no skill installation was performed.
- `markdownlint-cli2@0.23.3` passed for all 70 collection Markdown files in the
  isolated candidate, with no issues.
- Codex skill-creator `quick_validate.py` accepted clearml-yolo metadata.
- Additional YAML validation rejected duplicate keys and checked field types,
  name/folder agreement, name syntax and lengths, and allowed metadata keys.
- CY's Markdown/local-link checker, using the portable skill folder as its
  root, accepted SKILL.md and both changed references with no issues; local
  references remained inside the package and existed.
- VERSION `0.6.5` matches the dated CHANGELOG entry; the candidate remains on main
  with latest inspected origin/main as an ancestor and no existing `v0.6.5` tag.
- `git diff --check` passed. The no-impact architecture record passed plan and
  acceptance validation against this collection root.
- Documented setup ordering and guarantees were compared against current CY
  `adapters/yolo/assets.py`, `adapters/assets/preparation.py` and
  `adapters/integrations/batch_probe.py`. Public command and report schemas are
  unchanged by this correction; no new command example was introduced.

## Independent review and delivery scope

Fresh independent read-only review of the final CY and collection diffs found no
blocking code or documentation findings. The reviewer answered both appended
explanation-only cases without executing commands: success covered 10/10 expected
behavior groups and failure covered 7/7. These were native agent evaluations. This evidence does not establish a live OpenCode/model
invocation, GPU trial, native AMP numerical execution, real offline checkpoint
load or training/publication outcome. Parent verification owns those applicable
runtime checks and final source/version reconciliation.

The original old checkout's stale release-route/fallback prose was already
corrected on latest main; this isolated candidate preserves that correction.
CY canonical acceptance passed with a stable receipt: 4482 tests passed, 30 skipped
and 81 deselected. Final post-hook source installation of CY 0.44.2 with queue
0.1.5 passed normal resolution, metadata/import checks and all 518 ordinary queue
tests (seven physical cases deselected). The collection release follows its clean
main, consistent VERSION/changelog and annotated-tag atomic-push workflow. The
original pending work is preserved. No installed-skill export or package/asset
publication is part of this release.
