# clearml-yolo Git workspace warning guidance

## Intent and ownership

Document the advisory warning emitted when an experiment command cannot confirm
that its launch directory belongs to a Git working tree. Preserve command output,
workload authorization and the user's repository choices.

This task owns the clearml-yolo entrypoint, usage/evaluation references, this plan,
the adjacent architecture record, dated verification and collection release metadata.
The application implementation and tests belong to the companion clearml-yolo task.
Pre-existing changes in the shared collection checkout remain outside this worktree.

## Architecture

The [architecture record](architecture.json) records no runtime impact: this
collection change updates skill prose and release metadata only. It introduces no
runtime modules, executable checks, dependency edges or active host configuration.
The clearml-yolo repository owns application architecture and runtime verification.

## Tasks and acceptance

1. Validate the planning architecture record before editing the skill.
2. Explain launch cwd versus CY_HOME, supported Git working-tree forms, warning
   timing and nonfatal diagnostics. Add realistic explanation-only evaluation cases.
3. Reconcile every behavioral statement with final application source and evidence.
4. Validate frontmatter, duplicate keys, metadata, package-local links, collection
   Markdown, discovery and whitespace using MAINTENANCE.md and RELEASE.md.
5. Record explicit native-agent scenario results separately from source inspection
   and unavailable OpenCode/model execution; obtain applicable independent review.
6. Validate architecture acceptance, update VERSION/CHANGELOG for a patch release,
   and commit only owned files. Publish the collection after application delivery.

Documentation preparation can proceed beside application implementation. Final source
reconciliation and review depend on integration and checks; publication depends on
the application release. No delegated writers are needed within this bounded task.

## Status

Planning, skill edits, source reconciliation, package/document checks and fresh
independent review are complete. Collection 0.4.1 is accepted for commit;
publication follows verified application delivery. Results are recorded in the
[dated evidence](../../../verification/2026-10-20-git-workspace-warning.md).
