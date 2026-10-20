# OpenCode model routing and prompting

## Intent and scope

Upgrade the existing orchestration package and its companion skill-author guidance
for Qwen3.8-27B, DeepSeek-V4.1-Flash and GLM-5.3. Models are optional; choose by
capability, task evidence and availability. Preserve skill names, standalone
packaging, Plan boundaries, permissions, ownership and fresh final review.
Do not install skills or change live provider configuration.

The request spells the DeepSeek revision `0117`; official release documentation
identifies `1017`. Clarification is pending. Keep this distinction explicit and
leave example provider IDs unresolved rather than silently inventing a mapping.

## Architecture

The [architecture record](architecture.json) covers prose, inactive example
templates, evaluation cases and release metadata. These packages contain no
runtime module or executable dependency contract. Model/permission examples are
distribution templates, not active composition. No import checks are required.

Pre-existing ClearML/CVeta edits and the stable-cache plan are outside this task.
The parent owns every changed path. Read-only evaluators have no write ownership.

## Execution and acceptance

1. Inspect current guidance and primary model/host sources. Evaluate the old skill
   before rewriting. Record limitations of the baseline.
2. Validate the architecture record before implementation.
3. Rewrite routing, prompts, optional profiles, inactive agents and authoring
   compatibility together; retain dated historical reports.
4. Validate YAML/JSON, duplicate keys, metadata, local links/anchors, package
   boundaries, permission invariants, CLI discovery, Markdown and whitespace.
   Run representative fresh-context scenarios. Model settings are adapter
   contracts, not portable skill metadata.
5. After parent integration/checks, obtain fresh independent read-only review.
   Resolve findings and verify affected final content.
6. Update version/changelog and dated evidence. Follow RELEASE.md with a
   task-scoped commit, annotated Git tag and atomic push. Use a clean temporary
   checkout of the committed release if unrelated working changes remain.

Baseline evaluation can run independently of parent source research. Editing
depends on that evidence; final review depends on integration and checks.
No parallel writers are needed.

Commands: the discovery/lint/diff checks in RELEASE.md, package validation from
the authoring reference, and the installed architecture validator for `plan` and
`acceptance`. Live OpenCode/model execution is a separate check; do not infer it
from proxy evaluations or parser success.

## Status

- Inspection and old-skill evaluation complete; baseline exposure caveat recorded
  for the dated evidence.
- Implementation and package/documentation checks complete.
- Fresh independent review accepted the final scope; thirteen proxy tabletop
  scenarios passed. Target OpenCode/model execution remains untested.
- Architecture acceptance validated. Delivery follows RELEASE.md for 0.3.0,
  integrating three newer upstream ClearML releases in a clean checkout and
  preserving all original unrelated working edits. See the
  [dated evidence](../../../verification/2026-10-20-opencode-model-routing.md).
