# Maintain skills

This repository owns portable skill sources. Installed skills are distribution
copies, not authoring sources.

No local checkout path, private endpoint, credential value or machine capacity
belongs in these packages. Read machine-specific requirements from the user's
maintained environment guidance.

## Verification

From the collection root:

```bash
DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list
```

Obtain fresh independent review for substantive changes after integration and
behavioral checks.

When changing a skill package, validate frontmatter, unique keys and name/folder
agreement so the host can discover and load it. Follow the functional metadata
rules in the [package validation guide](skills/opencode-skill-author/references/validation.md).

Run the relevant package's evaluation prompts in fresh contexts. Separate routing
from explicit skill reading, and record observed results. For a live OpenCode run,
record host version, provider, resolved model, mode/effort, prompt, output and
artifact checks. If that runtime is unavailable, say so; native agent evaluation
and static checks do not establish DeepSeek performance.

## Source-only acceptance

Source maintenance does not require installation on the author's PC. When installation
is excluded, run list-only discovery and package checks against the source tree.
Evaluate routing in a fresh context supplied only the available names/descriptions;
evaluate instruction application separately using explicit package reads. Record
these as native source-based evaluations, including the host and observed outputs.
They do not prove automatic routing in OpenCode or performance on its target models.

Installed-copy acceptance applies only to a requested installation on its intended
host: compare that copy to its released source and verify actual host discovery and
runtime behavior there. Keep such work separate when outside the current scope.
A source-only release may report those outcomes as untested without treating local
installation as a prerequisite. Preserve dated historical evidence; label superseded
model guidance and link the maintained reference rather than rewriting old results.

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
