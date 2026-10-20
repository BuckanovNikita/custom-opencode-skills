# Changelog

Versions apply to the complete skill collection. Releases use annotated Git tags;
no packages or release assets are published.

## [Unreleased]

## [0.3.0] - 2026-10-20

- Route OpenCode work across DeepSeek-V4.1-Flash, optional GLM-5.3 engineering/review
  and optional Qwen3.8-27B visual or bounded analysis; retain one-model operation.
- Rewrite task prompts and provider-specific thinking guidance, with explicit model
  identity, image capability and fresh-review checks. Distinguish unverified `0117`
  from the official DeepSeek `1017` revision.
- Add inactive GLM engineer and Qwen visual analyst examples and evaluation cases;
  align the standalone skill-author package. Existing skill and agent names remain
  stable; examples require manual provider-ID mapping and are not activated.

## [0.2.12] - 2026-10-20

- Document ClearML dataset selectors for ground truth, CSV-only inventory, source
  provenance, local-input compatibility and deferred queue/replay resolution.
- Explain default detector, ONNX and terminal UI dependencies while preserving
  external source/system prerequisites and the separate development group.
- Add selector, media-path, queued-input and default-installation evaluation cases.

## [0.2.11] - 2026-10-20

- Point clearml-yolo native configuration, cache and publication guidance to its consolidated
  stage-zero specification; preserve existing commands and examples.

## [0.2.10] - 2026-10-19

- Explain stable experiment parameters and opt-in controller cache identity while
  retaining full executable replay settings.
- Document active `run`, selected-preset `run_overrides`, startup training parameters
  and publication of complete artifacts as soon as they are ready.
- Add configuration, cache-reuse and result-completion evaluation scenarios.

## [0.2.9] - 2026-10-19

- Explain per-job queue monitoring warnings, retained summaries and archive-aware
  warning counts while preserving strict execution and reservation behavior.
- Add partial-monitoring and shared-refresh-failure evaluation scenarios.

## [0.2.8] - 2026-10-16

- Document clearml-yolo per-user queue settings discovery and required filled fields,
  retaining explicit shared-file overrides for users and worker services.
- Explain eligible NFS coordination, effective local-lock settings, outage limits
  and preserved offline relocation; add setup and failure scenarios.

## [0.2.7] - 2026-10-16

- Align clearml-yolo usage with required shared TOML queue configuration, readable
  job references, reversible archives and updated terminal controls.
- Preserve exact machine provenance while documenting readable messages, reports
  and console output; add shared-user, archive and configuration-failure scenarios.

## [0.2.6] - 2026-10-16

- Document clearml-yolo's CSV-relative media resolution, literal detection labels,
  resolved-reference cache identity and canonical-name-only evaluation.
- Add explicit evaluation cases for conflicting working directories and unsupported
  prediction naming modes, with source/package checks and fresh Codex scenario evidence.

## [0.2.5] - 2026-10-15

- Align clearml-yolo guidance with its 0.31.0 interface: physical GPU
  labels, controls without tokens or reasons, per-device manual freeing and fixed
  used-VRAM admission strictly below 2 GiB with a fresh startup check.
- Add evaluation cases for manual freeing, the memory boundary, older interfaces
  and public GPU labels. Record independent documentation review and four
  reasoning-based scenario checks separately from live hardware/model evidence.

## [0.2.4] - 2026-10-15

- Document clearml-yolo 0.30.2 guardian-owned cleanup and post-training GPU
  reservation contraction; add handoff and cleanup-status evaluation cases.

## [0.2.3] - 2026-10-15

- Document clearml-yolo 0.30.1 queue worker logging defaults, verbosity controls,
  status heartbeat and retained pipeline logs; add diagnostic evaluation cases.

## [0.2.2] - 2026-10-15

- Align the cveta2 installation prerequisite with its Python 3.12/3.13 support
  window and add an installation-version evaluation case.

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
