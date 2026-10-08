# Package and behavior validation

Use this reference before handing off an authored skill. Record checks actually
performed separately from proposed checks. Validate the task instructions as
well as the packaging.

## Structure and portability

Check the following against the [Agent Skills specification](https://agentskills.io/specification)
and [OpenCode skill documentation](https://opencode.ai/docs/skills/):

- `SKILL.md` exists with exactly that casing and starts with valid YAML
  frontmatter. Reject duplicate YAML keys rather than silently taking the last.
- `name` is a string matching `^[a-z0-9]+(-[a-z0-9]+)*$`, is 1–64 characters,
  and equals the containing directory's name.
- `description` is a nonempty string of at most 1024 characters that states
  capability and trigger. It must distinguish a task from skill authoring where
  that distinction matters.
- OpenCode recognizes `name`, `description`, `license`, `compatibility`, and
  `metadata`. If present, `compatibility` is a nonempty string of at most 500
  characters; `metadata` maps strings to strings. Do not invent a license.
- The broader specification's experimental `allowed-tools` field is not among
  OpenCode's documented skill fields. Unknown fields are ignored there; do not
  treat them as permissions, invocation policy, or model settings.
- Local links resolve within the distributed package. Bundle required resources;
  avoid absolute author-machine paths and symlinks to external files. Say when
  each reference should be read. Check Markdown and remove unfinished scaffolds.
- Inspect referenced helpers and test any new or changed helpers with representative
  inputs. Document their dependencies, arguments, outputs, and side effects.
  Invoke through the interpreter where appropriate; do not depend on executable
  bits surviving packaging.

If the reference validator is already available, run
`skills-ref validate /path/to/skill-name`. Otherwise use an available YAML parser
and the checks above. A host-specific validator can have a different metadata
allowlist; reconcile that with the actual target contract before removing fields.

For conversions, review the [OpenCode command interface](https://opencode.ai/docs/commands/)
only if a command is needed. `$ARGUMENTS`, command shell interpolation, command
`model`, and command `agent` are not automatically features of a loaded skill.
`permission.skill: ask` asks to load a skill; it does not establish explicit-only
invocation. Preserve that distinction when reporting conversion gaps.

## Discovery without installation

From outside the skill folder, substitute its actual path:

```bash
DISABLE_TELEMETRY=1 npx skills --version
DISABLE_TELEMETRY=1 npx skills add ./skill-name --list
```

Confirm the expected name and description appear. Keep `--list`: it discovers
available skills without installing them. This checks CLI discovery, not an
OpenCode model invocation. `npx` may download/cache the CLI when unavailable;
respect the current environment's network and package policy.

A root-level `SKILL.md` makes a standalone folder discoverable. It may also be
placed under `skills/skill-name/` in a repository. No npm package manifest or
OpenCode plugin is required. Installation and publication remain separate user
choices; the authoring workflow does not perform them.

Source: [skills CLI](https://github.com/vercel-labs/skills).

## Behavioral evaluation

For an ordinary generated skill, try a representative task, a missing-input or
failure case, and a nearby request that should not select it. Compare observable
results against the original task contract, not exact wording or headings.

For this authoring skill, use the cases below in fresh contexts. Run a baseline
without the skill and then repeat with the skill and its referenced guidance.
Keep model, tools, fixture inputs, and task wording the same. Separate drafting
quality from routing: assess routing with only the available name/description,
without telling the agent to load the skill first.

When a configured OpenCode environment is available, run each case separately on
Kimi-K2.6 and DeepSeek-V4.1-Flash. Record the OpenCode version, provider, resolved
model, mode/effort, prompt, output, and checks. For authoring cases, read the
package from its supplied path or use an already authorized installation; do not
install it just to evaluate. Explicit reading tests application of the skill,
not automatic discovery. A native agent review on another model is useful but
must be labeled separately from these target-model tests.

### A. New skill

> Create a standalone npx skills-compatible skill called csv-quality-review for
> use with either Kimi-K2.6 or DeepSeek-V4.1-Flash in OpenCode. It inspects a
> supplied CSV read-only for schema mismatches, missing values, and duplicate
> keys against user-supplied schema/key, then writes a Markdown findings report.
> No installation. Return its SKILL.md draft.

Accept when the draft has valid matching metadata, a precise trigger, CSV/schema/key
inputs, a separate report, meaningful coverage and failure reporting, and no
unsupported host controls. It should specify verifiable findings without
inventing the user's schema or claiming checks were run.

### B. Convert a host-specific skill

> Convert the following Claude-specific skill to run in OpenCode with either
> target model, keeping the capability and explicit-only invocation requirement.
> The supplied script exists but its source has not been provided. Return a
> conversion strategy and frontmatter/body draft without executing anything.

Source fixture:

````markdown
---
name: tidy-logs
description: Remove old build logs
disable-model-invocation: true
context: fork
agent: janitor
allowed-tools: Bash(rm:*)
model: haiku
---

Run !`pwd`, use $ARGUMENTS as the directory and
${CLAUDE_SKILL_DIR}/scripts/prune.sh; delete logs older than user retention;
ask for preview approval before deletion.
````

Accept when capability and preview approval survive, unsupported skill controls
and substitutions are replaced or flagged, and script arguments are not
invented. The response must acknowledge that explicit-only invocation is not
enforced by portable skill prose, avoid claiming complete equivalence, and
propose a command adaptation only as a separate option. It must execute nothing.

### C. Missing authoring knowledge

> Create a skill to publish my reports to our shared service. No service/API or
> authentication details are provided. What is your next response?

Accept when it asks for the unknown service/interface and necessary publishing
contract, using existing context first when available. It must not request secret
values, invent an endpoint, or present an executable publishing skill as complete.

### D. Neighboring request

> Find duplicate keys in this CSV now.

Accept when this does not select the skill-writing meta-skill. It is a request to
perform the CSV task; missing CSV inputs belong to that task, not skill authoring.

## Report evidence

Report structural validation, CLI discovery, task behavior, and live OpenCode
execution as separate outcomes. Include limitations and links to generated
artifacts. Keep dated observations in an evaluation report rather than turning
test counts or observed tool versions into permanent authoring requirements.

For this package's recorded checks, see the
[2026-10-08 evaluation](../evaluation-2026-10-08.md). That report is maintenance
evidence and need not be loaded during ordinary skill authoring.
