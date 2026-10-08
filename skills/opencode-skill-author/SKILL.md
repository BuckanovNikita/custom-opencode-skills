---
name: opencode-skill-author
description: Create, revise, or convert npx skills-compatible SKILL.md packages for OpenCode with Kimi-K2.6 or DeepSeek-V4.1-Flash. Use for skill authoring, packaging, and portability requests; not for executing the task a skill describes or configuring model providers.
---

# Author portable OpenCode skills

Produce a self-contained skill package that either target model can follow in
OpenCode. Use one shared task procedure; model names do not select a model or
require a planner/worker arrangement.

## Establish the contract

1. Identify the task, intended users, trigger, required inputs, output, and evidence
   of completion. Read existing skill files, callers, and relevant task examples
   before revising or converting them.
2. Distinguish invocation inputs from missing authoring knowledge. A future user
   can supply a filename; an unknown service API can prevent writing a working
   publishing procedure. Inspect available context, then ask only for facts or
   decisions that materially affect correctness. Label incomplete drafts.
3. Honor the requested destination. Otherwise produce a standalone folder named
   after the skill in the working directory, outside agent discovery directories.
   Preserve existing files and invocation restrictions. Authoring alone does not
   request installation, publication, or configuration changes.

## Write the skill

Read [model guidance](references/model-guidance.md) before authoring for either
target model. Apply its prompting guidance; consult runtime notes only when a
request depends on model settings or provider compatibility.

- Start with the outcome, input requirements, and scope. Separate instructions
  from examples and supplied data with ordinary Markdown headings or fences.
- Describe concrete actions in dependency order where order matters. Give tools
  the necessary inputs, explain what result to inspect, and name the evidence
  required before moving on. Use the host's available tools and actual schemas.
- State the expected output shape and useful amount of detail. Include one small
  input/output example when prose alone leaves the contract ambiguous. Keep
  examples consistent with the rules.
- Define handling for missing essential inputs, unavailable tools, and partial
  results when relevant. Distinguish attempted work from verified completion.
  Preserve task-specific approvals without inventing unrelated approval stages.
- Keep essential decisions in `SKILL.md`; move conditional detail to directly
  linked references and say when to read each. Add scripts only for useful,
  repeatable automation, with dependencies, invocation, and side effects stated.
- Use concise task instructions. Avoid repeated emphatic rules, speculative
  model weaknesses, requests for hidden reasoning, or raw model control tokens.
  Ask for conclusions and verification evidence instead.

## Package and convert

Use this minimum layout and metadata; replace the example with the actual task:

```text
csv-quality-review/
└── SKILL.md
```

```yaml
---
name: csv-quality-review
description: Review CSV data against a supplied schema and key definition and produce a Markdown findings report. Use for CSV quality checks.
---
```

Names must match the containing folder, use lowercase letters/digits with single
hyphen separators, and contain 1–64 characters. Descriptions must contain 1–1024
characters and distinguish authoring or task triggers from nearby requests.
Keep optional metadata only when useful and supported; the exact contract and
checks are in [validation](references/validation.md).

For conversion, preserve capability, inputs, dependencies, outputs, and
authorization rules. Replace host-specific placeholders with explicit input
handling and paths relative to the actual skill directory. Inspect bundled
scripts before prescribing their arguments; do not invent flags.

OpenCode skill frontmatter does not implement Claude's `context`, `agent`,
`model`, or `disable-model-invocation` controls. Its command templates are a
different interface. Do not assume command argument substitution, shell
injection, or Codex-only tool names work inside a skill body. Document any
necessary host adaptation separately.

If explicit-only invocation or isolation cannot be preserved by the target skill
format, report the gap before calling the conversion complete. Body prose is not
host enforcement. An explicit OpenCode command may be an alternative, but changes
the deliverable and needs the user's agreement. Do not silently drop restrictions.

## Verify and hand off

Use [validation](references/validation.md) to check structure, references,
discovery, and representative behavior. Verify new helpers with realistic inputs
in an isolated workspace. A parser or `--list` pass does not prove model behavior.

Return the package path, material changes, checks performed, and remaining
limitations. State which host and model were actually tested; if neither target
model was available, provide runnable evaluation prompts and say so.
