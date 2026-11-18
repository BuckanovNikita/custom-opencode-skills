# Portable skill collection instructions

This repository owns portable skill sources; installed copies are exports.
Follow [MAINTENANCE.md](MAINTENANCE.md) for source ownership, metadata, links and
evaluation. Follow [RELEASE.md](RELEASE.md) for
versioning, changelog, checks, independent review and Git-tag-only release.

Keep `AGENTS.md` authoritative and `CLAUDE.md` a regular `@AGENTS.md` import adapter.
Keep machine-specific setup and credentials outside portable packages. The linked
maintenance and release procedures own package verification and releases; changes reach
`main` through the PR delivery below, except the release commit that the separately
authorized [release procedure](RELEASE.md#tag-and-push) pushes together with its tag.

Keep README.md in Russian and other instructions/documentation in English.

## Agent skills

### Issue tracker

Active issues and plans live in Linear (team `SIP`, project `custom-opencode-skills`). Delivery is a GitHub PR from a task branch: agents push the branch and open the PR, humans merge. Read [the tracker guidance](docs/agents/issue-tracker.md) before planning, picking up or delivering work.

### Triage labels

Readiness and category roles are Linear labels, separate from lifecycle state. See
[triage labels](docs/agents/triage-labels.md).
