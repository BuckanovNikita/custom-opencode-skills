# Evaluation evidence — 2026-10-08

This report records package checks and native-agent drafting exercises. It does
not establish execution quality on Kimi-K2.6, DeepSeek-V4.1-Flash, or OpenCode.

## Package checks

- The installed skill-creator frontmatter validator accepted the package.
- Additional checks accepted the OpenCode metadata allowlist, unique YAML keys,
  name/folder agreement, description length, local links, portable paths, and
  absence of unfinished scaffolds.
- `markdownlint-cli2` 0.23.3, using markdownlint 0.41.1, reported no issues.
  The line-length rule was disabled to allow source URLs and long descriptions.
- `npx skills` 1.5.23 discovered exactly `opencode-skill-author` through
  `skills add ./opencode-skill-author --list`. No skill installation was performed.
- All 14 unique external source URLs returned HTTP 200. This is availability
  evidence; the relevant provider and format statements were also read.

## Drafting and routing exercise

An independent agent drafted responses before the package existed. A fresh agent
then used the completed entrypoint, model guidance, and structural validation
guidance on the same four requests. The latter did not receive the evaluation
rubrics or baseline outputs. Both were requested as GPT-6.1 Sol with medium effort
through native Codex delegation; the tool did not expose runtime verification of
those model settings. Neither exercise used Kimi or DeepSeek.

| Case | Baseline observation | With-skill observation |
| --- | --- | --- |
| New CSV review skill | Produced a read-only procedure, required schema/key inputs, and a separate findings report. | Preserved those behaviors and included a concrete CSV/report example, readback, and partial-coverage reporting. |
| Claude-specific conversion | Flagged missing host enforcement and required helper inspection. | Produced an explicitly incomplete conversion, preserved preview approval, and prescribed no invented helper flags. |
| Unknown publishing service | Asked for the service/API and publishing contract without requesting secrets. | Distinguished missing service knowledge from future invocation inputs and asked for the missing contract. |
| Execute a CSV task now | Declined to select the authoring meta-skill. | Reached the same routing decision from the name and description. |

Baseline excerpt: “An explicit-only requirement needs host enforcement.”

With-skill excerpt: “This is an incomplete conversion draft, not an installable
operational replacement.”

The parent inspected the actual generated drafts and decisions. Both generated
`SKILL.md` drafts passed frontmatter validation; the pruning draft intentionally
remained non-operational because the helper and invocation enforcement were
unavailable. These checks do not establish that either generated procedure ran.

The baseline already handled these cases sensibly. This small exercise supports
the package's clarity and scope preservation; it does not demonstrate a measured
improvement or a model-specific success rate. Routing was assessed explicitly,
not through automatic OpenCode skill selection.

## Remaining coverage

Live OpenCode discovery and execution with each target model were not tested.
No provider configuration, API credentials, or paid model calls were used.
Reproducible prompts and acceptance criteria are in
[validation](references/validation.md). Temporary evaluation drafts were reviewed
and removed after verification; the delivered package contains one authored skill.

## Repository packaging check — 2026-10-08

The package was moved into `corp-skills/skills/opencode-skill-author/`; SHA-256
comparison confirmed that all four original files survived the move unchanged
before this evidence section was appended. From the repository root,
`npx skills add . --list` found exactly one skill. Frontmatter validation, all
seven repository-local Markdown links, and Markdown linting passed. Discovery
used list mode only and did not install the skill into an agent environment.
