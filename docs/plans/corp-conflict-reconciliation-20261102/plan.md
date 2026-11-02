# Reconcile collection history and publish shared entrypoints

## Goal and ownership

Reconcile the committed collection history against current published main in an independent clean main checkout. Preserve the original branch, index and all unfinished ClearML authoring files. Add the reviewed shared AGENTS.md and regular CLAUDE import convention. Publish a compatible documentation patch through the existing Git-tag-only release workflow after checks and fresh independent review.

## Architecture

See [architecture.json](architecture.json). Inspected impact is documentation-only: no executable interface, dependency or effect-owner changes. Current published tool contracts win over older unreleased descriptions; completed historical evidence is retained as history.

## Tasks

- [x] Inspect original dirty state, committed divergence, native maintenance/release rules and current upstream procedure.
- [x] Reconcile all committed history in isolation; preserve newer published skill content.
- [x] Prepare patch version/changelog, validate metadata/references/discovery and documentation.
- [ ] Obtain fresh independent review and validate architecture acceptance.
- [ ] Publish reviewed main and annotated tag atomically, verify remote identity.

## Initial decisions

The original local managed-dataset skill changes already match published main. The earlier local Russian-reference instructions were explicitly unreleased; upstream now documents the published CY 0.43.0+ contract and must not be rolled back. Preserve committed local history through a non-destructive merge while resolving procedural content in favor of current published behavior. Leave uncommitted experimental source outside this release. The added root entrypoints are compatible documentation and require a patch version above current 0.7.0.
