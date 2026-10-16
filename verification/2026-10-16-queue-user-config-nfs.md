# Queue configuration and NFS guidance — 2026-10-16

## Planned scope and acceptance

This companion updates the clearml-yolo skill entrypoint, queue usage instructions
and evaluation cases for required per-user TOML configuration, explicit shared-file
overrides and supported NFS coordination. Source reconciliation uses the application's
`master` implementation under native feature `035-queue-user-config-nfs`, delivered
in application 0.34.0. Collection 0.2.8 carries the matching guidance. Existing completed evidence remains
unchanged, and no installed skill is replaced.

The [architecture record](2026-10-16-queue-user-config-nfs-architecture.json) records
the inspected documentation-only scope before usage edits. No runtime module,
dependency, composition target or executable checking tool changes in this companion.
The application owns runtime architecture and locking acceptance.

Acceptance requires final source/help reconciliation, isolated configuration examples,
frontmatter and packaged-link validation, collection Markdown checks, non-installing
skill discovery, diff checks and architecture-record validation. Fresh independent
review and scenario evaluation follow parent integration and verification. These
checks establish no real NFS mount, remote-host execution or live OpenCode model result.

## Executed baseline and package checks

The released application 0.33.0 loader was inspected with its fixed-bootstrap binding
and namespace checks. An isolated CLI subprocess with a filled default user file,
without `CY_QUEUE_CONFIG`, reproduced the previous requirement to export that variable.
It failed before creating either queue root. This establishes the compatibility change;
it is a CLI baseline, not an automatic skill-routing evaluation. The fixture was removed.

- The no-impact architecture record passed plan validation before editing usage prose.
- Installed skill-creator validation passed for the clearml-yolo package.
- A duplicate-rejecting YAML loader checked frontmatter types, recognized keys,
  folder/name agreement and all four packaged local links.
- Skills CLI 1.7.1 discovered all four collection packages using `add . --list` with
  telemetry disabled. The command did not install skills.
- Collection markdownlint-cli2 0.23.3 checked 39 Markdown files with zero issues.
- `git diff --check` passed.

## Implemented interface and example checks

Inspected the updated application settings loader, CLI help/parser and read-only
configuration branch. The usage contract matches default HOME/XDG discovery, explicit
configuration precedence, all three required TOML fields, absolute path validation,
effective NFS superblock-option validation and unchanged bootstrap/binding behavior.

An isolated CLI subprocess suite passed the following examples without creating any
metadata or coordination roots: help with no configuration; missing and incomplete
configuration; default HOME lookup; absolute, empty and relative XDG lookup; explicit
shared-file precedence; metadata-only `--root`; rejection of a relative explicit file;
and human/JSON `config` output. These subprocesses imported the updated application
source through the prepared application environment. Temporary fixtures were removed.
They used local storage and exercised no queue submission, worker or host binding.

Source reconciliation identified the empty-HOME edge: Python's `Path.home()` returns
the filesystem root when the real `HOME` variable is empty. This was reported to the
application owner and corrected: the loader now uses the account database when HOME
is absent or empty. The application's final settings suite passed 56 tests, including
both account-home fallback cases; the isolated CLI examples passed again afterward.

Fresh independent review accepted source correspondence and documentation architecture
with no severity findings. Native-agent scenarios correctly handled per-user defaults,
shared override across users/services, required settings, eligible NFS, rejected unsafe
locking, no local NFS availability and preserved binding/legacy evidence. Independent
read-only probes, architecture acceptance and diff checks passed. This is explicit-skill
native-agent evaluation, not automatic-routing or live OpenCode/target-model evidence.
Application 0.34.0 passed the full suite (2,959 passed, 30 skipped, seven deselected),
all required feature/release hooks and independent application review. Its wheel and
source distribution each passed isolated command/configuration checks. Its annotated
Git tag and master branch were published and remote references verified before this
companion release. The collection's release metadata changes only VERSION and CHANGELOG;
the final source diff remains documentation-only. Final Markdown checks covered all
39 maintained documents, with no issues. Collection publication uses its annotated
0.2.8 Git tag; no installed skill, GitHub Release or registry package is created.
No actual NFS filesystem is available in this execution environment. No NFS mount,
service, account, production queue state or installed skill was changed.
