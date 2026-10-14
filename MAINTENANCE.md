# Maintain tool usage skills

This repository owns portable skill sources. Tool repositories own executable
interfaces and public contracts. Installed skills are distribution copies, not
authoring sources. A user-facing feature is complete only after its affected
usage guidance is current and verified, or an inspected no-change reason is recorded.

## Ownership and triggers

| Tool | Canonical package | Native workflow owner |
| --- | --- | --- |
| cveta2 | [skills/cveta2](skills/cveta2/SKILL.md) | [Engineering documentation stage](https://github.com/BuckanovNikita/cveta2/blob/main/.specify/memory/engineering.md#portable-usage-skill) |
| clearml-yolo | [skills/clearml-yolo](skills/clearml-yolo/SKILL.md) | [Agent documentation stage](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/agent-workflow.md#portable-usage-skill-maintenance) |

Check skill impact for additions, fixes, removals and changed defaults affecting
commands, public API, configuration, authentication, input/output formats, model
support, resource use, publication, errors or recovery. Internal refactors with
unchanged usage can record an explicit no-change decision. Treat current source,
schema and command output as evidence; specifications alone describe intent.

## Feature-to-skill workflow

1. During the tool's native planning stage, inspect the relevant package and record
   affected files/sections. Add skill update and validation tasks dependent on the
   implementation. Preserve completed history when resuming a feature or bug.
2. After implementation, reconcile that scope against the final diff. Update the
   entrypoint only for essential routing or decisions; put detailed options and
   examples in its references. Keep required resources inside the package.
3. Cover the new or changed behavior in its evaluation cases, including a failure
   or missing-input case when relevant. Preserve task boundaries and existing
   authorization. An example must not imply that config composition proves a live
   service, training run or publication succeeded.
4. Verify documented interfaces against the implemented version. Use isolated
   local fixtures for examples that write files; run service workloads only when
   the task authorizes them. Validate the package using the checks below.
5. Record tool branch/version/commit, changed skill paths, checks, review and
   limitations in dated evidence. Link the companion changes across repositories.
   Missing source access or required checks remain open documentation tasks;
   do not label the feature complete with stale usage guidance.
6. Follow each repository's commit/release workflow within existing authorization.
   Publish the tool implementation before claiming its new interface is released;
   label unreleased examples explicitly if they must land first. Publish this
   collection through [RELEASE.md](RELEASE.md). Reinstallation is a separate task.

No local checkout path, private endpoint, credential value or machine capacity
belongs in these packages. Read machine-specific requirements from the user's
maintained environment guidance.

## Verification

From the collection root:

```bash
DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list
npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'
git diff --check
```

Validate frontmatter, unique keys, name/folder agreement and packaged local links
using the [package validation guide](skills/opencode-skill-author/references/validation.md).
Check every changed command/field against tool help or source. Tool-repository doc
tests alone do not validate these external packages. Obtain fresh independent
review for substantive changes after integration and checks.

Run the relevant package's evaluation prompts in fresh contexts. Separate routing
from explicit skill reading, and record observed results. For a live OpenCode run,
record host version, provider, resolved model, mode/effort, prompt, output and
artifact checks. If that runtime is unavailable, say so; native agent evaluation
and static checks do not establish DeepSeek performance.

## DeepSeek V4.1 Flash authoring

Use outcome-first task contracts: required inputs, observed facts, conditional
actions, result shape and acceptance evidence. Delimit example data, use actual
tool schemas, ask only for missing material inputs, and distinguish requested,
attempted and verified actions. Keep detailed reference material conditional.
These are engineering practices, not a vendor-proven optimal prompt formula.

The collection's default remains DeepSeek V4.1 Flash with thinking and maximum
supported effort, preserving an explicit model choice. Select that through the
actual host adapter; skill prose does not set runtime parameters. Do not request
private reasoning, insert raw control tokens, or apply old R1-specific advice.
Protocol history preservation belongs to the adapter. Current provider facts and
sources are in [model guidance](skills/opencode-skill-author/references/model-guidance.md).
