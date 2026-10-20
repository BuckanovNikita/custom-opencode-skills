# OpenCode model routing verification — 2026-10-20

## Scope and architecture

Upgrade orchestration and the companion authoring package for DeepSeek-V4.1-Flash,
GLM-5.3 and Qwen3.8-27B. Preserve one-model operation, Plan restrictions, task/file
ownership, native tool boundaries and fresh independent review. Add inactive GLM
engineering and Qwen visual-analysis examples. No live configuration or installed
skill is changed. Existing ClearML/CVeta edits and the stable-cache plan are excluded.

The [plan](../docs/plans/2026-10-20-opencode-model-routing/plan.md) and
[architecture record](../docs/plans/2026-10-20-opencode-model-routing/architecture.json)
cover prose, inactive templates and release metadata. Plan validation passed
before implementation. There are no changed runtime modules or import contracts.

Primary-source identity and selection evidence is in the
[dated assessment](../skills/opencode-orchestration/references/model-selection-2026-10-20.md).
Qwen38-27B resolves to the official Qwen3.8-27B name. The requested DeepSeek `0117`
suffix was not verified; official sources identify `1017`. Examples retain explicit
provider placeholders and require identity resolution before dependent dispatch.
Role preferences are engineering inferences, not locally measured winners.

## Baseline

A bounded read-only native agent evaluated regression/plot and one-model Plan
scenarios before source edits. The agent accidentally read a short skill excerpt
before recording its initial answers. Therefore this is an old-guidance application
and static coverage baseline, not an uncontaminated no-guidance control.

It found that the old package selected DeepSeek for all roles, referenced Kimi and
Qwen3-Coder-Next, and lacked Qwen3.8/GLM guidance or an explicit image-delivery gate.
The old guidance already preserved Plan boundaries, ownership and fresh review.
The proxy response initially implied another model was needed for independent
review; the agent identified that as its own error after reading the full skill.
The revised cases explicitly test fresh same-model review.

The baseline requested GPT-6.1 Sol at medium effort with a fresh context, no write
ownership and no further delegation. Requested settings are not independent runtime
telemetry. It did not run OpenCode or the target models.

## Acceptance ledger

- Primary-source inspection and architecture plan validation: complete.
- Package structure, links, examples, discovery, Markdown and diff checks: passed.
- Fresh-context scenarios and independent final review: passed.
- Architecture acceptance: passed; release scope is collection version 0.3.0.

## Parent checks

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` discovered all four
  collection skills. This was listing, not installation.
- Collection `markdownlint-cli2@0.23.3` passed with zero issues.
- A duplicate-rejecting YAML parser checked both changed skill entrypoints and
  six inactive agent examples, including metadata, matching package names, modes,
  explicit provider placeholders, dispatch allowlists and denied worker delegation.
  Read-only agents deny shell/edits and use allowlisted reads.
- Local link/anchor checks passed for both complete OpenCode packages, README,
  changelog, plan and this report. All required skill resources remain within
  their standalone package; no external symlinks were introduced.
- Parsed the unchanged Plan JSON and checked model/Plan selection and absence
  of permission overrides. The fetched official OpenCode schema accepted a
  combined Plan/agent configuration after replacing model placeholders in memory
  with schema-known `deepseek/deepseek-flash`. This checks configuration shape,
  not actual GLM/Qwen IDs, model availability or provider behavior.
- `git diff --check` passed. Parent inspection excluded all pre-existing
  ClearML/CVeta/stable-cache changes from the release scope.

## Independent acceptance

After parent integration and checks, a fresh read-only reviewer requested as
GPT-6.1 Sol at high effort returned **ship**, with no findings. It inspected the
complete task-owned diff, new files, inactive permissions and no-impact architecture
classification. It independently reran the package/link/example validator and
`git diff --check`, and spot-checked official Qwen, GLM and DeepSeek runtime sources.
It reviewed discovery/lint evidence without claiming to rerun those commands.

It applied orchestration cases A–J and authoring cases E–G as tabletop scenarios.
All thirteen met their stated acceptance criteria: bounded role routing, Plan
restrictions, exact-pin mismatch, optional-model fallback, missing images, fresh
review after GLM authorship, adapter protocol diagnosis, conflicting findings,
resource ownership, neighboring requests and authoring/host boundaries.

These are fresh-context instruction evaluations, not executed repository fixtures
on the selected target models. No clean no-guidance control or repeated
target-model prompt comparison was completed; no measured prompting improvement
is claimed. Requested native agent settings are not independent runtime telemetry.

The final architecture acceptance validator passed. The task-owned scope includes
both OpenCode packages, README, version/changelog, this report and the adjacent
plan/record. Historical reports and unrelated working changes are preserved.

## Release integration

Fetching upstream revealed three already-published ClearML documentation commits
through collection 0.2.12, ahead of the inspected local base 0.2.9. Integration was performed in a clean temporary checkout, resolving only the
VERSION/CHANGELOG conflicts: version 0.3.0 retains every upstream release note.
The six agent templates and both OpenCode packages are unchanged from the
independently reviewed task commit. Upstream ClearML files retain their published
content. Final collection lint checked 53 files with zero issues; discovery found
all four skills; package/schema/link checks and architecture acceptance passed.
A preservation hash check in the original workspace observed concurrent changes
to two unrelated CVeta files, so it cannot establish byte-for-byte stability.
Those files were not edited or staged by this task and remain outside its commit.
Git branch/tag verification belongs to the delivery handoff; no GitHub Release
objects or package publication are part of this workflow.

## Limits

OpenCode is not installed in this environment. No target-provider calls, runtime
permission tests, live image delivery, reasoning-history round trips or model
quality comparisons have been performed. Prompt recipes and role allocation need
representative target-host evaluation. Static/proxy checks cannot prove the trio
outperforms a single model, or that an endpoint resolves a pinned revision.
