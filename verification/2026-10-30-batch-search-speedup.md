# Batch search speedup guidance, 2026-10-30

This documentation-only change accompanies CY feature 006's
[speedup amendment](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/006-batch-training/plan.md#approved-speedup-amendment-2026-10-30).
The isolated collection source starts at main
`b4fdd473ef85571fa81f75b47dd0ece8a10e1659`, version `0.6.6`.
The application candidate starts at CY release `0.44.3`, commit
`02e2d50ad9f3dd6ea619f69adfe5ffc45c32f361`; the parent owns integration and the
final application release identity. Collection patch `0.6.7` is prepared.
New guidance checks installed help/source support, without promising the interface
in older packages or claiming an unreleased application version is published.

## Scope and decisions

Changed source paths are `skills/clearml-yolo/SKILL.md`, its `references/usage.md`
and `references/evaluation.md`, VERSION, CHANGELOG.md and this evidence. The
[architecture record](2026-10-30-batch-search-speedup-architecture.json) documents
the inspected lack of runtime-module impact. No executable helper, dependency,
installed skill copy, original source checkout or user workload is changed.

Guidance covers memory-guided candidate selection with unchanged full trial
requirements; actual failure bounds and independent confirmation; numerical
recovery's dependence on the candidates actually observed; schema-3 strategy
records and legacy schema/key preservation; independent searches on idle matching
selected GPUs; automatic and capped concurrency; `--jobs 1`; cache, failure and
cancellation semantics; unchanged single-GPU measurement and linear extrapolation.
Physical multi-GPU execution is explicitly unverified for this release, as accepted
by the user. CPU concurrency does not establish GPU acceleration or distributed
training correctness.

## Validation and review

The shared architecture validator passed both plan and acceptance phases for the
documentation-only record. Collection discovery with the pinned skills CLI found
all four skills, without installation. Pinned Markdown lint passed all 72 collection
Markdown files. The Codex quick metadata validator and extra duplicate-key/type/name/
length checks passed. CY's local-link checker passed the skill entrypoint and both
changed references with their package root, plus five changed collection documents
with the collection root; both runs reported zero issues. `git diff --check` passed.

The implemented shared CLI parser accepted the selected-model/device example with
automatic jobs, `--jobs 1` and `--jobs 2`; help contains the new option. Resolved
settings retain five warm-up updates, ten measured updates and `memory_guided_v1`.
Help/parsing does not execute native work. The three new evaluation prompts are
explanation-only and authorize no workload, mutation or installation. Fresh
independent review exercised all three prompts after integration. No live OpenCode or
target-model evaluation is claimed.

The parent owns application native benchmarks, canonical verification, queue
compatibility, final independent review and release. No companion commit, tag,
push or installation has occurred at this checkpoint.

## Fresh independent review

The fresh native agent `review_portable_speedup` reviewed the full tracked diff and
both added evidence files with no write ownership or further delegation. Its final
verdict was **ship**, with no unresolved findings. It identified overly broad failure
isolation wording; the parent clarified that ordinary measurement/result failures
leave peers running, while unrecoverable supervision failures can stop the invocation
and preserve already accepted results. The reviewer inspected the corrected wording.

Explicit skill-reading prompt outcomes:

1. Independent search: proposed `cy-batch-estimate --imgsz 960 --models yolo11n,yolo11s
   --devices 0,1 --jobs 2` without executing; explained default concurrency, serial mode,
   per-case affinity/deduplication, full trials, cached relaunches, cleanup isolation
   and extrapolated multi-GPU reporting. No native speed claim.
2. Numerical recovery: legacy can freshly confirm four and guided can freshly confirm
   two for the supplied histories; neither may invent batch-four evidence for guided
   search or treat nonfinite failure as a memory boundary. Failed confirmation remains
   unusable; numerical diagnostics are retained.
3. Migration: preserved legacy replay and keys, required explicit schema-3 strategies,
   rejected unsupported schema/strategy combinations and explained read-only preservation
   and independent compatibility requirements without relabelling old trial histories.

The reviewer independently passed both architecture-validator phases, changed Markdown
and local-link checks, and diff whitespace; reviewed existing discovery/lint logs and
metadata/parser evidence; and inspected implementation and regression-test sources.
This is native-agent explicit-reading evaluation, not automatic-routing evaluation or
live OpenCode/target-model performance. Actual model/effort telemetry was not exposed.
Native timing and physical multi-GPU outcomes remain unverified.

## Queue admission threshold follow-up

The user additionally requested a local queue used-VRAM threshold. Queue source
starts at `397753c123f379ade50f06a2babf5e4b1476b9d2`, version `0.1.5`, with
compatible patch `0.1.6` prepared separately. The portable guide documents the
optional `[queue].max_used_memory_gib` setting and its 2.0 default, finite positive
numeric validation, effective-value diagnostics, strict byte boundary, fresh startup
recheck and unchanged holds/manual-free semantics. Its generic 6.0 example does not
encode a private machine setting. Direct CY checks and removed worker/Hydra options
remain distinct. Existing fixed-threshold evaluation expectations are reconciled,
and a new explanation-only configuration/boundary prompt covers the changed contract.

The implementation owner reported 199 focused checks passing, including default and
configured boundaries at admission and guardian startup, invalid TOML values, missing
telemetry, frozen launch payloads, actual CLI composition and owned subprocess launch.
The portable maintainer inspected the matching settings model, public config output
and boundary implementation. An isolated source-CLI example verified the documented
6.0 configuration, the omitted-field 2.0 default, string-value rejection and no queue
state creation. Updated metadata validation, local links, architecture phases,
all 72 collection Markdown files and whitespace checks passed. Normal dependency
resolution installed the proposed CY 0.44.4 and queue 0.1.6 sources together in an
isolated environment. The full queue suite passed 544 tests with seven physical-GPU tests deselected and 19 existing
Pydantic warnings in 133.74 seconds; queue Ruff/format, strict typing, both import
contracts, help, package consistency and explicit source/installed origins and
versions passed. This verifies the proposed pair; final committed identities and
the corresponding installation refresh remain publication gates.

## Fresh queue and portable follow-up review

The independent native agent `review_queue_memory` reviewed the queue implementation
and the integrated portable follow-up with a **ship** verdict and no findings.
It independently passed 88 focused queue tests, confirmed the 73-file frozen queue
source/test/check/metadata inventory, checked package consistency, and passed changed
portable Markdown, local links and architecture acceptance.

The reviewer exercised the new explanation-only threshold prompt with explicit skill
reading. Its response preserved the existing required TOML name/roots, used the
generic 6.0 example, retained the 2.0 default, required memory strictly below 6144 MiB
and rejected missing telemetry and invalid nonpositive/nonfinite/non-numeric values.
It distinguished the value loaded by `config` from the threshold already held by a
running worker/guardian, required an owned-worker restart for new settings and kept
old launch thresholds frozen. It explained matching worker/execution upgrades,
existing manual-free and reservation semantics, the absence of a memory-fit guarantee,
and unchanged direct CY checks; removed worker flags and Hydra overrides remained
invalid. No command, mutation or workload was performed for the prompt.

Both batch-search and queue follow-up documentation scopes now have fresh independent
review with no unresolved findings. This remains native-agent explicit-reading
assessment, not automatic routing or live OpenCode/target-model performance.
Publication of collection 0.6.7 waits for the actual CY/queue committed-pair refresh
and verified public code refs. No tag, push or installed-skill replacement is part of
this commit-preparation checkpoint.
