# Tool usage skills and maintenance

## Scope and design

Add portable `cveta2` and `clearml-yolo` usage skills for OpenCode, following the
existing authoring contract and DeepSeek V4.1 Flash guidance. Each package owns
its task routing, prerequisites, command/configuration examples, side effects,
failure handling and completion evidence. Inspect current default-branch source
before documenting an interface; do not turn feature-branch intent into support.

Extend each tool's existing documentation stage to update its corresponding
package in this repository when a user-facing contract changes. Keep one canonical
skill source, record a justified no-change decision for internal-only changes,
and surface unavailable cross-repository updates as incomplete documentation.
Do not edit installed skills or vendor workflow templates.

## Architecture

[Architecture record](architecture.json): documentation-only, with no runtime,
dependency, executable checker or application configuration changes. Skill
instructions describe existing interfaces; they do not own service credentials,
machine setup or execution permissions. Each installed package must resolve its
required local references without another skill installed.

## Work and dependencies

- [x] Inspect collection conventions, source repositories and current model docs.
- [x] Identify independent source inspection for each tool; parent owns integration.
- [x] Validate the architecture planning record before authoring.
- [x] Author and source-check both skill packages; add runnable evaluation cases.
- [x] Update both native documentation workflows and their dated no-impact records.
- [x] Update collection catalog, maintenance guidance, changelog and minor version.
- [x] Run discovery, Markdown, frontmatter, links and representative scenario checks.
- [x] Inspect integrated diffs, obtain fresh independent review, and reconcile records.

Release follows this accepted snapshot through each repository's native workflow;
final refs and outcomes are recorded in the dated verification report and handoff.

Source inspection and package authoring can proceed independently per tool.
Cross-repository policy changes follow the agreed canonical package paths.
Verification, final review, commits and release follow integration sequentially.

## Acceptance

Run the commands in `RELEASE.md`, including skills CLI discovery and Markdown lint;
include `docs/**/*.md` in lint coverage for this task. Check all four skill packages'
frontmatter and local reference containment. Verify documented options against
source and available read-only command helps/config composition. Run fresh-context
success, missing-input and near-miss scenarios per package. Record native Codex
evaluation separately from live DeepSeek/OpenCode evaluation; do not invent a
configured provider or run production training/uploads for documentation checks.

Tool repositories use their own documentation and commit checks. Final acceptance
requires the complete task-owned diff, no-impact acceptance validation and fresh
independent review. Record results and limitations in dated verification evidence.
