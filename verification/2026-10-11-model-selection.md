# Quality-first model selection verification — 2026-10-11

## Scope

Updated both skills, their current guidance, and inactive examples to default to
DeepSeek-V4.1-Flash with thinking and maximum supported effort. Added a dated
benchmark assessment and a Plan startup example. Preserved historical evidence,
standalone skill packaging, permission contracts of existing agents, and explicit
user model choices. No installed host configuration was changed.

This is a compatible guidance correction released as collection version 0.1.1.
Provider/model placeholders remain unresolved in the distributed examples.

## Evidence and baseline

Primary-source research compared the three existing target models. The
[assessment](../skills/opencode-orchestration/references/model-selection-2026-10-11.md)
separates published OpenCode results, other agent frameworks, and benchmark
versions. Selecting one default for every role is an engineering inference;
matched planning, review, and skill-authoring benchmarks were not found.

A fresh read-only baseline agent inspected the original package before edits.
It found an unranked Kimi-primary/Qwen-implementer split and no explicit Plan-mode
branch in the authoring skill. Missing-model disclosure and fresh-session review
were already covered and were preserved.

After editing, a separate fresh agent evaluated six tabletop scenarios:

| Scenario | Initial result and disposition |
| --- | --- |
| Quality-first Plan followed by Python/ML implementation | Found one stale Kimi/Qwen split in the Python/ML example; corrected it to role-based ownership with DeepSeek as default |
| Default model or maximum effort unavailable | Requires disclosure, actual adapter support, and no silent substitution |
| Same-model implementation and review | Requires a fresh reviewer session, raw evidence, and separate permissions; acknowledges correlated errors |
| Skill authoring in Plan | Requires a proposal before package writes or executable validation |
| Direct CSV task instead of skill authoring | Excludes the authoring meta-skill |
| Explicit Kimi choice or representative evidence favoring Qwen | Preserves the selection and each model's supported runtime behavior |

The startup JSON was then simplified to select the model and initial Plan agent
without changing existing permissions. The final text allows a host-authorized
plan artifact while keeping implementation outside Plan.

Research requested GPT-6.1 Sol at high effort; baseline and scenario agents requested
GPT-6.1 Sol at medium effort, all with fresh contexts and no write ownership or
further delegation. Requested settings are not independent runtime telemetry.
These checks assess instructions, not live behavior of the three target models.

## Parent verification

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: both skills discovered;
  this was listing, not installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'`:
  no Markdown lint issues in the checked package.
- Parsed Markdown and validated local links/anchors and skill-package boundaries.
- Parsed YAML with duplicate-key rejection; checked skill metadata, matching names,
  four agent model placeholders, modes, and worker/reviewer permission invariants.
- Parsed the Plan JSON and checked its model selection and absence of permission
  overrides. The official configuration schema rejects its two intentional model
  placeholders; replacing only those values with the schema-listed
  `deepseek/deepseek-flash` in memory passes validation. This does not confirm
  runtime model availability, version resolution, or provider effort support.
- Direct Python retrieval of the schema returned HTTP 403; retrieval with curl
  succeeded. Validation used the schema's declared JSON Schema draft.
- `git diff --check`: no whitespace errors.

## Independent acceptance

After parent integration and verification, a fresh read-only reviewer requested
as GPT-6.1 Sol at high effort returned `ship` with no blocking findings. It inspected
the complete change, all three new files, model routing, agent permissions, and the
minimal Plan configuration. It independently checked the cited benchmark results,
hosted effort control, and documented Plan permissions against primary sources.

The reviewer reran all six tabletop scenarios against the final instructions and
confirmed the stale Python/ML routing was gone. The Plan transition preserves the
default model and authorization boundary, while same-model review remains fresh.
It reviewed the parent's structural/schema/discovery/lint evidence rather than
claiming to rerun those checks. Its live-runtime limitations match those below.
This acceptance record was appended after that review.

## Limits

OpenCode was not installed in the verification environment. No provider calls,
paid model evaluations, live Plan transitions, or effective runtime permission
checks were performed. Structural validation and tabletop review do not establish
that DeepSeek is superior on a particular repository task. The example requires
model-ID replacement and adapter-specific effort configuration before use.
