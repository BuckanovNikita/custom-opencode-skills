# Source guidance verification, 2026-11-04

## Scope and implementation evidence

Candidate base: collection main `5e1cb87`, version 0.7.2. Original dirty authoring
checkout is excluded; its unfinished ClearML edits and old version metadata were
not incorporated. Current published CY origin/master is `8be76e4`, with 0.45.0
release commit `850557c`. Its estimator has no `--force` option. An independent
read-only investigation also inspected the unfinished force feature source based
on `84166d5`: option declaration, GPU inventory/probe policy, SearchSettings,
key generation and recommendation reporting. These are unpublished working changes;
the collection describes them only as unreleased, gated on actual installed support.
It neither implements nor publishes that CY feature.

The published core excludes per-GPU saved results not divisible by eight. The old
A=8/B=5 evaluation's global15 recommendation contradicted that policy. The corrected
case uses A=16/B=8 and global24, and explicitly rejects unaligned historical B=5.
Historical schema replay remains supported and is preserved. Superseded model
assessments retain their original results with prominent current-guidance links.
Numeric training batch selection is distinguished from estimator search limits.

## Executed source checks

- Planning architecture validator: exit 0 before edits.
- Pinned skills 1.7.1 `add . --list`: exit 0, four expected packages, no installation.
  The CLI prints generic “installing non-interactively” agent-detection wording even
  in list mode; this invocation ends with available-package output only.
- Pinned markdownlint-cli2 0.23.3: exit 0, 85 Markdown files, zero issues before
  adding this evidence; final rerun includes it.
- Unique YAML keys, supported frontmatter fields, names/folders, name/description
  limits and absence of package symlinks: all four packages passed.
- Built-in skill-creator quick validator: ClearML package passed.
- Existing CY Markdown checker: nine changed/new prose files, then all 18 skill
  entrypoint/reference files; zero link/structure issues.
- Initial plain-system-Python checker invocation could not import Pydantic. The
  actual checks above used an existing interpreter containing its dependencies;
  no environment or package was installed.
- Git whitespace check: exit 0. Final scope includes new untracked plan/evidence
  and architecture record in addition to the tracked documentation/release diff.

## Fresh native evaluation

Requested native model/effort: gpt-6.1-sol / medium; runtime realization is not exposed.
All children were bounded, read-only and forbidden from further delegation.

`routing_evaluation` received the four source names/descriptions (two initial
paraphrases were replaced with verbatim source descriptions before the final result),
seven requests, no skill bodies and no intended decisions. Observed choices:

| Request | Observed selection |
| --- | --- |
| Explain cy-batch-estimate --list | clearml-yolo |
| Create a CSV duplicate-review skill | opencode-skill-author |
| Find CSV duplicate keys now | None |
| Export CVAT project using cveta2 without publication | cveta2 |
| Explain generic YOLO architecture | None |
| Coordinate Python/ML implementation and review in OpenCode | opencode-orchestration |
| Fix cveta2 repository source | None |

All seven match the description boundaries. This is native semantic routing from
provided metadata, not automatic discovery or execution inside OpenCode.

`usage_evaluation` received the source entrypoint and three realistic requests,
without the evaluation rubric. It read relevant usage sections. Observed answers:

1. A16/B8 across A,A,B yields extrapolated global24/perGPU8 absent exact evidence;
   sources must match. B5 is excluded without rounding; remeasure or choose an
   explicit batch, otherwise null lookup stops.
2. CY0.45.0 lacks estimator force. Newer source tolerates unattributed activity but
   rejects visible foreign compute; hidden activity remains possible. Forced results
   retain background-sensitive provenance, compatible training reuse, fresh checks
   and older-reader compatibility warnings, with no uncontended-maximum guarantee.
3. FCOS numeric global16 on two GPUs uses model.train.batch=16, eight per GPU,
   bypasses lookup and requires sufficient memory. Its additional dataset-size wording
   was not established as a universal backend rule; final independent review corrected
   the source to retain backend/dataset constraints without imposing a minimum
   dataset length. Search-limit16 caps search,
   which can test8 then16 plus confirmation; it does not select one exact trial.

The evaluator additionally retained default output counts1..N, single-GPU fresh
confirmation, extrapolation and separate physical DDP acceptance. All three meet
the scoped explanation criteria. It ran no workload or installed-help command.

## Acceptance boundaries

The user explicitly excludes installation on this PC. No installed skill, shared
instruction, host setting, adapter, service or process was changed. Cross-host
export synchronization is inapplicable: this is a portable source-only collection
with no requested registered export changes. Target-host installed-copy validation,
live OpenCode routing and target-model execution remain untested and outside this
task. No GPU/DDP, ClearML publication or measured model-quality acceptance follows
from source checks or native evaluations. Those limits do not require a local
installation to release corrected portable documentation.

Final independent review, architecture acceptance and publication are recorded
below after execution. Publication follows RELEASE.md with Git refs only.

## Final review correction

Independent reviewer `final_review` inspected the final tracked/untracked scope,
architecture, evidence and actual published/unfinished source contracts. It found
one minor overstatement: numeric batches do not universally require a minimum
dataset length. The parent corrected usage and evaluation wording to preserve
backend/dataset constraints and available memory, recorded the narrower observation
above, and reran Markdown/link/whitespace checks successfully. All other reviewed
contracts were accepted; scoped re-review of this correction follows below.

## Accepted source release candidate

Scoped independent re-review returned **ship** with no remaining actionable findings.
Final diff reconciliation includes only the ten existing documentation/version paths
and this task's three plan/architecture/evidence files; no executable or installed
export changes. Architecture acceptance validation passed. Final Markdown lint
checked 86 files with zero issues; changed-file links and Git whitespace passed.
Version/changelog agree at 0.7.3. Commit/tag publication remains the post-commit
release step and its actual receipt is appended after remote verification.
