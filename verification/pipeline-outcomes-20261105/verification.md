# Pipeline outcome guidance verification

Date: 2026-11-05. Source-only maintenance; installed exports are outside this task.

CY 0.45.1 adds warning completion after verified training, NA training counts, visible report
exclusions and pipeline outcome artifacts. Its Git release was verified before changing the
examples from unreleased to versioned guidance.

The usage and evaluation references were checked against CY result models, report adapters,
pipeline orchestration and ClearML finalization. A fresh independent native reviewer accepted
the prose as accurate. The evaluation case requires inspecting outcomes rather than treating
exit zero as proof of complete publication; unknown counts must not be replaced with zeros.

List-only skills discovery found all four packages. Scoped Markdown lint passed. Full
collection Markdown lint passed (87 files). A fresh native evaluator returned the expected
answer for the new failure prompt and passed every criterion: outcome inspection, unavailable
counts, exclusion rules, strict standalone/interruption behavior and unreleased qualification.
Package YAML duplicate-key/name/description checks, 14 relative targets and four heading
fragments passed. No automatic routing or target-model performance claim is made. Claude and
Codex continue to discover the same canonical package; no host-specific settings changed.

## Final application delivery evidence

Implementation commit: `42bff25`. Release commit: `66279a6e47a1bcb1dd64e1445f4ae5f4c28ea67d`.
CY remote master and annotated `v0.45.1` were verified at that commit after atomic push.
Only root version metadata and generated changelog changed in the release commit.

Canonical CY verification passed Ruff, strict mypy, Vulture, import contracts and 4789 tests;
30 skipped and 81 opt-in tests deselected. Documentation validation covered 241 Markdown
files with zero issues. Architecture acceptance and fresh independent review passed.
A live CPU YOLO/ClearML run preserved best.pt after an injected metrics failure and read back
completed task status with a completed_with_warnings outcome and status message. Task-owned
remote resources were cleaned; this does not establish model quality or GPU/NCCL behavior.

Queue source `694c12f9c2a1a0064284a69d010e846108d43875` remained current at release. Ordinary
editable installation of the final CY 0.45.1 source with queue 0.1.8 passed dependency
resolution and pip check. Source and installed versions matched. The final pair passed
583 queue tests with seven opt-in tests deselected. Queue static/import checks had passed
on the same unchanged queue source; current/proposed pair runs also passed before release.

The collection release is 0.7.4. Its source-only publication changes no installed host exports,
settings or routing. All four skills remain discoverable; Markdown, package/link checks and
independent behavioral evaluation passed. No model/provider performance claim is made.
