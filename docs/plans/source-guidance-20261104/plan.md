# Reconcile portable source guidance

User scope: fix legacy guidance, batch documentation and routing acceptance in the
source collection. Do not install this skill set on this PC. Preserve the original
dirty authoring checkout. Follow MAINTENANCE.md and RELEASE.md.

## Architecture

[architecture.json](architecture.json) records documentation-only impact after
inspection of current published skill packages and CY option/training contracts.
No executable module, dependency, import contract or host configuration changes.

## Tasks

- [x] Inspect current published main, original edits and published tool interfaces.
- [x] Clarify historical guidance and numeric training batch versus estimation.
- [x] Validate metadata, packaged links, Markdown and list-only discovery.
- [x] Evaluate routing from descriptions and explanation behavior in fresh contexts.
- [x] Reconcile final scope, obtain independent review and validate acceptance.
- [ ] Publish a patch commit and annotated Git tag; verify remote refs.

Independent read-only contract investigation can overlap parent documentation work.
Behavior evaluation follows edits; final independent review follows parent checks.
Live OpenCode execution and installed-copy acceptance belong to a separate task on
an intended target host, not to this source-only correction.
