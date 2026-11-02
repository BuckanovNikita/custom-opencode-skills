# Instruction migration checks — 2026-11-02

## Scope and preservation

The task standardizes agent instruction discovery only. Existing application,
service, generated workflow and unrelated source changes are preserved. No runtime
API, database, dependency or deployment changes are made. The implementation checks below preceded commit/release activity; release
outcomes are reported by the coordinating task separately. No deployment or
skill installation is part of this migration.

Pre-existing skill source changes are unrelated and remain unstaged. New root
instructions route to the existing owning maintenance and safety guidance.

## Executed checks

- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'` — PASS: 60 Markdown files, zero issues.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` — PASS: four expected portable skills listed; list mode does not install skills.

Planning and acceptance record validation use the shared architecture validator
with this repository as the explicit root. All root adapter and changed-document
relative links were inspected separately; Git whitespace checks cover owned paths.

## Limits and remaining integration

File checks establish physical instruction discovery structure, not actual loaded
model context. Fresh Claude/Codex sessions, Windows discovery, settings parity,
parent integration, independent final review and authorized commit/release remain
with the coordinating task. Relevant application/runtime suites were not rerun
because no application behavior changed. Missing required release checks must
remain acceptance gaps rather than being inferred from these documentation checks.

## Independent review

The coordinating parent reported fresh `project_review` verdict **SHIP** with no
blocking defects on 2026-11-02. The reviewer validated all twelve project no-impact
records and inspected the documentation evidence across the integrated migration.
No runtime acceptance is inferred from that review. Authorized repository commit
and release gates remain in force; failed gates must be reported separately.

## Release gate outcome

A current fetch succeeded. The checkout has divergent pre-existing history
(local VERSION 0.3.0 versus origin/main tagged v0.6.9), plus unrelated dirty source.
RELEASE.md's clean-tree/main/ancestor requirements block tagging and publication.
Its commit paragraph permits exact intended paths while preserving other changes;
only the reviewed instruction migration is eligible for a contributor commit.
No version field, changelog, tag or remote history is rewritten for this task.
