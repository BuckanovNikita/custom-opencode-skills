# Simplified GPU control guidance — 2026-10-15

## Scope and source

Update the canonical clearml-yolo entrypoint, usage reference and evaluation prompts
for physical-index presentation, argument-free enable/disable/free controls, per-device
manual reservation release, and the fixed automatic admission limit below 2 GiB.
The collection source starts at `ce33ce5` (version `0.2.4`); the application source
starts at `9355693`. Application implementation and acceptance are recorded in its
[feature amendment](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/031-test-quality-gpu-controls/gpu-control-simplification).

The [architecture record](2026-10-15-gpu-control-simplification-architecture.json)
classifies this companion scope as documentation-only. Existing source has no
package edits to preserve. Installed skill copies are outside this task.
`VERSION` and the dated changelog prepare collection patch `0.2.5`; stable skill
names, layout and invocation remain unchanged. The usage heading and changelog now
identify the intended application interface as `clearml-yolo 0.31.0`. Application
release hooks were in progress at that checkpoint; completed delivery is recorded
below after the parent verified the final references.

## Verification

- Inspected the updated application parser, public presentation, control adapter,
  generation locks and guardian launch gate against the usage/evaluation changes.
  Physical indexes and known short labels select controls. Free/release advances
  one generation without telemetry, process cleanup or queue-root access; current
  reservations and holds remain exclusive while stale unlocked metadata does not.
- Parsed eight documented list/control/worker command forms with the application
  parser. All pass without executing workers or mutating controls. Four removed
  reason/token/force/threshold argument forms reject with parser exit 2.
- `quick_validate.py` from the available Codex skill-creator validates both the
  portable package and the project-owned end-to-end skill.
- Duplicate-rejecting safe YAML validation passes for both skills: allowed fields,
  name/folder agreement, description bounds and package-local reference boundaries.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` discovers all four
  collection packages. This is discovery without installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'` passes across 36 collection files.
- Nine changed application Markdown files pass markdownlint with `MD013` disabled,
  matching prior documentation checks for existing long-line prose. The initial
  default-rule run reported line-length issues; no repository lint configuration
  was changed.
- Local file and heading-fragment targets were checked across the changed application
  documents, portable package and this evidence. No missing target was found.
- `git diff --check` passes in both repositories for this workstream's paths.
- After preparing `0.2.5`, reran full collection Markdown lint (36 files), discovery
  (four packages), strict metadata/package-boundary validation and skill validation.
  All pass. All 56 collection-local file/heading links resolve. VERSION matches the
  dated changelog. At that initial check, the usage heading still identified the next
  application release; the later version-label update is recorded below.
- The reconciled no-impact architecture record passes both plan and acceptance
  validation, including VERSION/CHANGELOG in its task scope. This structural check
  does not establish runtime or hardware acceptance.
- After naming the target interface `clearml-yolo 0.31.0`, reran full collection
  Markdown lint (36 files), discovery (four packages), strict frontmatter and package
  boundaries, all 56 local links/anchors, skill validation and both architecture
  phases. All pass. Collection `0.2.5` metadata agrees with the dated changelog;
  application delivery remains pending actual final commit/tag references.

The application end-to-end skill and prerequisites now include manual freeing,
generation fencing, public GPU labels and fixed startup memory checks. Documentation
validation does not establish those native hardware outcomes. Application source
delivery is recorded below separately from native physical acceptance.

## Independent review and scenario evaluation

The fresh native reviewer `/root/docs_review` returned **ship, no blockers**, as
reported by the parent. Its source spotchecks matched the documentation. The reviewer
also passed four reasoning-based usage scenarios in its fresh context:

| Scenario | Observed result |
| --- | --- |
| User declares a GPU free despite existing queue evidence | Uses manual free without token, reason, added memory/process checks or signals. |
| A live current allocation uses less than 2 GiB | Keeps that allocation exclusive until explicitly freed. |
| A new job follows manual freeing but now uses at least 2 GiB | Leaves the new job pending because its fresh memory admission fails. |
| CUDA visibility remaps submission indexes | Keeps physical GPU operator indexes distinct from logical submission indexes. |

This records the reviewer's documentation/source reasoning, not executed queue
workloads, automatic skill routing, target-model performance or hardware acceptance.

## Release and evaluation limits

The parent verified application annotated tag `v0.31.0` at
`7a4759876b57a511db071287a7eb708a916f35dc`, whose source parent is
`c5b19fb9f9e2471d77fce91dfd1724dc83935760`. Full source and version-commit hooks
passed. Wheel/source archives and isolated installed-wheel smoke checks passed for
the exact tagged tree; all dependency metadata stayed unchanged. The atomic branch
and Git-tag push succeeded, and remote branch/tag/peeled references match locally.

Collection patch `0.2.5` is prepared for its checked commit and annotated Git-tag
delivery. The application release evidence will record the resulting collection
commit and verified remote refs after publication. Installed copies remain unchanged.
New portable evaluation
prompts cover manual freeing despite live evidence, the threshold boundary, old-interface
rejection and public labels. The four fresh-context reasoning checks above passed;
the remaining acceptance prompts are expectations rather than recorded passing results.
No OpenCode executable is available. Native Codex review cannot establish OpenCode or
DeepSeek performance. No GPU workload, ClearML publication, real multi-user/NFS or
physical concurrency evidence is claimed by these documentation checks.
