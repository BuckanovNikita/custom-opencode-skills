# Aligned automatic batch guidance, 2026-11-02

The isolated collection source starts at main
`83982d2`, version `0.6.8`; patch `0.6.9` accompanies the CY batch-alignment fix.
The CY parent owns implementation, final source identity and application release.
The guidance checks installed strategy support before promising aligned behavior.

## Scope and architecture

Changed package paths are `skills/clearml-yolo/SKILL.md`,
`skills/clearml-yolo/references/usage.md` and
`skills/clearml-yolo/references/evaluation.md`. Collection version, changelog and
this dated evidence accompany the change. The
[architecture record](2026-11-02-batch-alignment-architecture.json) records the
inspected lack of runtime-module or executable-contract impact.
Original dirty source worktrees and installed skills remain unchanged.

The guidance describes `memory_guided_aligned_v1`, starts at 8/16 per GPU,
aligned candidate/refinement/confirmation, floor-rounded ceilings, invalid
sub-eight ceilings, no aligned capacity and unchanged explicit batches.
It retains historical replay/keys and excludes unaligned results from automatic
lookup without inventing measurements. Schema 3 remains explicit and the new
strategy gets a distinct key. No measured throughput improvement is claimed.

## Verification

Plan and acceptance validation passed for the documentation-only architecture
record. Pinned collection discovery (`skills@1.7.1 add . --list`) found all four
skills without installation. Pinned `markdownlint-cli2@0.23.3` passed all 74
collection Markdown files. The Codex metadata validator passed the changed package;
additional YAML validation confirmed unique keys, allowed fields, types, matching
name/folder and length limits. CY's read-only Markdown/local-link checker passed the
three changed package documents with the package as root and the two changed
collection Markdown files with the collection as root. `git diff --check` passed.

The implemented CY source was inspected for the aligned strategy name, default,
start/minimum, grid rounding, historical strategies and unaligned-lookup exclusion.
No live OpenCode or target-model execution is claimed. New evaluation prompts are
explanation-only and authorize no mutation, installation or workload. The parent
owns CY canonical checks, queue compatibility, runtime architecture and native
evidence limitations.

## Fresh independent review and behavioral evaluation

The fresh native reviewer `/root/final_review` inspected the integrated CY, queue
and portable changes and returned **ship**, with no blocking findings. It
independently reran 214 CY policy, strategy and recovery tests, then evaluated the
portable explanation cases: ceiling 31 floors to 24 per GPU; two identical GPUs
can produce an extrapolated global 48; 28 and 31 are not candidates; capacity
failure at 8 yields `no_capacity`; an explicit numeric training batch 5 retains
existing behavior. Historical confirmed batch 11 remains readable but cannot be
rounded or used automatically, and numerical recovery requires fresh complete
confirmation. This is native-agent review and explanation evaluation, not live
OpenCode or named target-model execution. No measured speedup, physical multi-GPU
acceptance or target-hardware result follows from it.

Documentation and architecture checks were rerun after recording the review.
Collection release 0.6.9 is prepared locally; publication waits for the companion
CY release. Installed skills and original source worktrees remain untouched.
