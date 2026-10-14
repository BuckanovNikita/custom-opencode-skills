# Changelog

Versions apply to the complete skill collection. Releases use annotated Git tags;
no packages or release assets are published.

## [Unreleased]

## [0.2.1] - 2026-10-15

- Document clearml-yolo 0.29.0 physical GPU controls, per-owner holds, audited
  whole-allocation release and supervision preflight; add portable evaluation cases.

## [0.2.0] - 2026-10-14

### Added tool usage skills

- Portable cveta2 and clearml-yolo usage skills with source-grounded examples,
  side-effect boundaries, failure handling and representative evaluation prompts.
- A cross-repository maintenance workflow linking user-facing tool changes to
  skill updates, validation and dated acceptance evidence.

### Changed maintenance guidance

- Extend collection checks to planning documentation and document outcome-first
  tool prompting for DeepSeek V4.1 Flash without unsupported runtime controls.

## [0.1.1] - 2026-10-11

### Changed

- Default all covered OpenCode roles and skill authoring to DeepSeek-V4.1-Flash
  with thinking and maximum supported effort; exclude price/token-based routing
  and replace the fixed Kimi-primary/Qwen-implementer split.
- Document the published benchmark basis and comparison limits, preserving Kimi
  and Qwen as explicit alternatives without claiming a universal winner.
- Clarify Plan-mode restrictions and the transition to authorized execution;
  include an inactive Plan startup configuration and same-model fresh-review guidance.

## [0.1.0] - 2026-10-08

### Added

- OpenCode skill authoring for Kimi-K2.6 and DeepSeek-V4.1-Flash, with model
  guidance, portability checks, and dated evaluation evidence.
- Python/ML orchestration for Kimi-K2.6, DeepSeek-V4.1-Flash, and
  Qwen3-Coder-Next, with context controls, verification guidance, and inactive
  agent examples.
- Repository-wide semantic versioning with a version file and a documented
  validation, commit, and Git-tag-only release procedure.
