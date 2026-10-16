# Readable shared queue guidance — 2026-10-16

## Source and scope

Updated the canonical clearml-yolo skill and its usage/evaluation references against
application branch `feat/readable-shared-queue`, initially based on 0.32.0. The final
source preserved the separately released 0.32.1 timeout correction and was published
as application **0.33.0** before this companion release. The implementation is governed
by [feature 034](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.33.0/specs/034-readable-shared-queue)
and its dated acceptance records. The remote application branch and annotated tag
were verified against their local references.

The change covers required `CY_QUEUE_CONFIG`, explicit metadata/coordination roots,
host binding and conservative relocation, readable stable job references, owner-only
archive/restore, TUI controls, machine-output opt-in and retained exact provenance.
README and historical evidence remain intact. No installed skill is replaced.

The adjacent [architecture record](2026-10-16-readable-shared-queue-architecture.json)
classifies this companion as prose/release-metadata-only after inspecting its actual
scope. Initial prose changes preceded persistence of the record; it is validated
before companion delivery and release metadata preparation. No executable module,
dependency, composition target or checking tool changes.

## Baseline and executed validation

The prior usage references returned raw IDs and did not require shared configuration.
Executing the former `CY_QUEUE_ROOT`-only listing recipe against the new application
failed for missing `CY_QUEUE_CONFIG`. This reproduced stale guidance; it was not a
fresh-context model baseline or a failed training run.

- Actual CLI help verified `config`, `submit --label/--json`, archive targets and list
  filters. Source inspection checked additional options, TUI bindings and exact
  metadata preservation.
- An isolated TOML fixture exercised read-only JSON settings, metadata override,
  agreeing/conflicting legacy roots and absence of created metadata/coordination
  directories. Temporary resources were cleaned up.
- Installed skill-creator validation passed. A duplicate-rejecting YAML loader
  verified frontmatter types, folder/name agreement and four package-local links.
- Skills CLI 1.7.1 `add . --list` with telemetry disabled discovered the package
  without installing it. Collection markdownlint-cli2 0.23.3 passed before this
  evidence file was added; the final check is recorded below.
- `git diff --check` passed for the companion and application documentation.
- The adjacent no-impact architecture record passed plan validation before preparing
  `VERSION` and `CHANGELOG.md` for collection 0.2.7.
- Final package Markdown validation including this evidence checked 38 files with
  zero issues; companion `git diff --check` remained clean.

## Independent evaluation and delivery

A fresh native reviewer read the skill and current application interfaces, applied
nine usage scenarios, and returned **ship** after corrections. The scenarios covered
shared configuration, conflicting roots, archive ownership and recovery visibility,
readable references, provenance, and live-versus-static verification boundaries.
The reviewer also executed read-only help/configuration checks. This is explicit
skill reading and reasoned scenario evaluation in the current agent host; it does
not establish automatic routing or live OpenCode/DeepSeek performance.

The review corrected an overbroad raw-log claim: invocation `stdout.log` and
`stderr.log` retain original bytes; a queue's combined child log can receive the
readable mirror during invocation capture. Current references identify the original
capture files for exact diagnostic recovery. No documentation findings remain open.
Static and scenario checks do not establish physical GPUs, NFS or ClearML publication.

Collection **0.2.7** is the guidance correction for application **0.33.0**, following
the existing Git-tag-only release policy. Application acceptance includes 2,927 routine
tests, real two-user ownership/worker checks, CPU and single-GPU publication, exact
provenance recovery, independent review and both installed distribution checks. Those
runtime outcomes belong to the application evidence, not the skill's scenario evaluation.

The collection's final Markdown, package discovery, frontmatter, local-link, diff and
no-impact architecture checks passed. The application delivery record records the
collection's final tag/push verification. No installed skill was replaced; no GitHub
Release object, release asset or registry package was published.
