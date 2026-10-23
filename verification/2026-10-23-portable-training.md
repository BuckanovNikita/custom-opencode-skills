# CY portable training skill verification, 2026-10-23

The companion implementation is CY feature `003-portable-training`, with the
separate source package cy-queue. Initial checks below preceded CY acceptance;
the implementation release and collection release preparation are recorded at the
end. Skill reinstallation was not requested or performed.

Task-owned edits cover source-first installation into one environment, coordinated
offline queue upgrade and standalone train submission, bare Agent replay, matching
non-YOLO assets, proxy fallback and offline recovery. Existing uncommitted ClearML
cache guidance and its planning files were preserved and excluded from ownership.
The documentation-only [architecture record](2026-10-23-portable-training-architecture.json)
validates successfully.

Executed checks:

- Skills CLI 1.7.1 discovery with `add . --list` found the package; no installation.
- Markdownlint CLI2 0.23.3 checked the collection's 50 Markdown files with zero issues.
- The reference skill validator passed. Independent validation rejected duplicate
  YAML keys, checked name/description/metadata constraints and verified all five
  packaged local links stay inside this portable package and exist.
- Current CY and queue source/help matched documented command names, configuration
  fields, replay ordering, native assets and offline handling.
- Fresh independent native-agent review returned `ship` with no blocking findings
  in the task-owned skill changes. The requested model/effort was gpt-6.1-sol/high;
  realized model/effort was not exposed by the collaboration tool.

The reviewer actually applied three supplied prompts. For offline queue upgrade it
preserved history/locks, required stopped workers and changed-identity resubmission,
and produced the `submit --command train --gpus 1 -- ...` template without running it.
For a missing gated DEIM backbone it retained the requested initialization and used
the missing-asset report's URL/destination/override, declining to invent an unknown
preset or substitute random/full-detector weights. For a bare Agent entrypoint it
explained early canonical restoration, same-task reuse, absent versus malformed
configuration and logical CUDA numbering without queue setup.

Description-based routing selected the skill for CY queue/Agent prompts and for
DEIM in the supplied CY context, excluding unrelated CSV-only work. The reviewer's
first read exposed the complete skill, so this was not a blinded routing experiment.
Explicit reading/application and source inspection were observed; live
OpenCode/DeepSeek execution was not performed. Static checks and this native-agent
evaluation do not establish performance on an unavailable target runtime.

## Integrated release candidate

The collection's remote main advanced while this task was in progress. The final
candidate uses published collection `v0.4.2`, commit
`f6148927336d83cac3cdc59dec68a44b6064a32b`, and prepares patch `0.4.3`.
The original checkout remains at `44426a6` with all its working edits intact;
publication is prepared in an isolated clean clone. The three-file delta relative
to the saved pre-task working versions and reviewed corrections to directly affected
installation guidance were integrated. Previously
published cache guidance, current ground-truth guidance and Git-directory warnings
are retained from remote main; unrelated local edits and planning files are excluded.

Candidate checks were rerun on 2026-10-23:

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` found all four expected
  skills, including clearml-yolo, without installing them.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'` checked 60 Markdown files with zero issues.
- Strict YAML metadata validation passed for all four skills, including an
  executable duplicate-key rejection check. Skill Creator's reference validator
  passed for clearml-yolo.
- Markdown/local-link checks passed across all 60 files. Separate package-root
  checks confirmed all clearml-yolo local links stay within its distributed package.
- Documentation-only architecture planning and acceptance validation passed after
  reconciling the seven task-owned paths, including release metadata and evidence.
- `git diff --check` passed.

Fresh independent review of this integrated candidate found stale installation
paragraphs inherited from the prior release: they still assigned the Textual queue UI
to CY and required a manually prepared DEIMv2 checkout. Those paragraphs and their
evaluation case now assign the command/UI/NVML dependency to separately installed
cy-queue, describe automatic approved source resolution for `model.source_dir=null`,
and retain explicit validated checkouts and external dependency/TensorRT requirements.
CY and queue package metadata plus the execution asset resolver confirm these contracts.
Markdown, local-link, metadata, package and architecture checks were rerun successfully
after this correction. Fresh independent re-review returned `SHIP` for the corrected
integrated candidate, with no blocking findings. The earlier native-agent application
evaluation above separately covers the task's behavior prompts.

## Implementation delivery and collection release preparation

Accepted collection candidate commit `05b515655c468a2a4d58ec71ce065fce4d7959a9`
was prepared locally before implementation publication. CY was then released as
[`v0.38.0`](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.38.0): feature
commit `ad6a8ec81901801c7bb434beed67405da81551ab`, release commit
`8caafcdcb4b93bb905cbe3d80508d8fa1db061ca`, annotated tag object
`0336f9e27cb4b9e38740dc1bccd28888ae3278bb`. Independent remote-ref inspection
confirmed the tag peels to that release commit and remote master matches it.
The implementation owner reported that feature and release hooks passed their
full suites, then refreshed the active source-first installation with CY `0.38.0`
and queue `0.1.0`; dependency consistency and commands/resources outside the
checkout passed. Those execution checks belong to the CY implementation evidence,
separate from this collection's documentation checks.

This evidence-only follow-up prepares collection `0.4.3` publication after the
implementation release, with the reviewed skill content unchanged. Collection
release follows the clean-tree annotated-tag and atomic-push procedure in
[RELEASE.md](../RELEASE.md). No skill installation or export is part of this delivery.
