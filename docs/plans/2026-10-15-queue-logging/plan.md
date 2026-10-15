# clearml-yolo queue logging guidance

Document the queue worker's default INFO diagnostics, selectable verbosity,
periodic status and separate pipeline log destination. Reconcile every option and
path with the implemented clearml-yolo interface before publishing. Preserve the
existing skill name, discovery metadata, invocation boundaries and installed copies.

Architecture: [record](architecture.json). The inspected skill and application
queue entrypoint, runtime and guardian show that this companion change affects
only prose, evaluation cases and release evidence. No executable module, dependency
contract, composition target or checking tool changes in this collection.

The application implementation is the dependency for final command checks and
source reconciliation; its published release is the dependency for this collection's
patch release. The parent coordinates fresh independent evaluation/review. This
bounded workstream owns only the companion collection and does not delegate.

- [x] Inspect the package, application interfaces and existing changes.
- [x] Record the documentation-only architecture scope and validation commands.
- [x] Update concise usage guidance and logging evaluation cases.
- [x] Validate metadata/local links, Markdown, discovery and actual CLI help.
- [x] Record implemented-source reconciliation, behavioral evaluation and review evidence.
- [x] Reconcile the application release version and commit before companion publication.
- [ ] Publish the collection patch after application delivery and verify Git refs.

Validation uses the collection's maintained `MAINTENANCE.md` and `RELEASE.md`
commands: `skills@1.7.1 add . --list`, `markdownlint-cli2@0.23.3`, frontmatter
validation with duplicate-key rejection, contained local-link checks,
`git diff --check`, and architecture plan/acceptance validation. Check actual
`cy-queue worker --help`; no queue submission or training is part of the examples'
validation. Record actual checks in
[dated evidence](../../../verification/2026-10-15-queue-logging.md).
