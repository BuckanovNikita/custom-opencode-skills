# clearml-yolo input correctness guidance — 2026-10-15

## Scope and source

Updated the authoritative `skills/clearml-yolo/references/usage.md` and
`references/evaluation.md` after inspecting the corrected storage, application ports,
canonical evaluation guards and prediction provenance in clearml-yolo master, based on
commit `813ef01` (0.31.1), integrated as source commit
`bdf21a63be95151759c58d2d701735b548c4baaa`. Application delivery is recorded below. No installed skill was replaced.

The guidance covers CSV-relative execution paths, literal labels and empty backgrounds,
cache version 2 resolved-reference identity, exact threshold round trips, canonical-name
evaluation and hash-bound naming provenance. Raw stem/path exports remain usable for
export; evaluation rejects incompatible naming and unknown image identifiers.
This is prose-only scope under the adjacent architecture record.

## Validation

- Skills CLI 1.7.1 discovery with telemetry disabled and `--list` passed; the package
  appears without installation.
- markdownlint-cli2 0.23.3 passed across the collection (37 files at the final packaging check).
- Codex's existing skill validator passed the package. An additional unique-key YAML
  loader verified frontmatter, metadata types, name/folder agreement and all three
  package-local links; no external author-machine resources are required.
- The application documentation checker passed all three package Markdown files and
  their local links. Source inspection confirms every changed option/field.
- Installed application acceptance covers comparison rejection of stem/path and raw
  prediction configuration accepting name/stem/path. Native acceptance covers relative
  CSV execution from conflicting directories, cache reuse and downloaded lexical rows.
  These are application checks, separate from skill packaging or model behavior.

## Fresh explicit skill evaluation

A fresh native Codex subagent read the package and both references, then answered:

1. A CSV has relative media paths and labels `NA`, `NULL`, `nan`, `None`, `0001`,
   Unicode and blank backgrounds; the launch directory contains conflicting images.
   It correctly selected media relative to the CSV, preserved lexical source lineage,
   distinguished empty background cells and literal labels, described cache identity
   and required an actual checkpoint before execution.
2. The user asks to score a stem export or compare with path naming. It rejected both
   evaluation modes, prescribed regeneration with canonical names, preserved raw
   exports/sidecars, and required membership validation for historical absent-mode
   provenance. It did not propose dropping metadata to bypass checks.

Both answers matched the implemented contract without claiming a workload ran. This
was explicit skill application in a native Codex context, not automatic routing,
OpenCode/DeepSeek execution, a separate target-host installation or a quality benchmark.
No live target-model claim is made. The application owns its native evidence and limits.

## Independent acceptance

Fresh read-only reviewer `/root/docs_acceptance_review` returned **ship**, with no
actionable findings in the companion source. It independently validated package links,
the no-impact architecture record and source-contract accuracy, and checked the
distinction between explicit Codex scenarios and unavailable target-model execution.
Collection 0.2.6 contains these compatible guidance corrections. The application was
published before this collection release; the companion tag identifies this evidence.

## Application delivery — 2026-10-16

Application [v0.32.0](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.32.0)
was published at version commit
[`3e79bf00a6b1915a6ff7ad7f5361df33881be91d`](https://github.com/BuckanovNikita/clearml-yolo/commit/3e79bf00a6b1915a6ff7ad7f5361df33881be91d).
The atomic push and subsequent remote query verified the master commit, annotated
tag object `4bd11af35e8e9c53010511d103a7d253c598df25` and its commit target.
The checked release workflow passed; final wheel and source-archive verification
matched all 180 accepted runtime files with unchanged runtime requirements.
The application retains dated native/package/review evidence and its existing
five-GPU and multi-host NFS acceptance gaps. No deployment or package upload occurred.
