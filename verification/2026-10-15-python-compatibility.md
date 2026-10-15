# cveta2 Python prerequisite verification

Date: 2026-10-15. Application change: cveta2 feature
`specs/007-python-support-window`, implemented in contributor commit `7f91109`
and release commit `7b23114` (v0.10.0), based on source commit `ebf6c70`.
The application package and lock require `>=3.12,<3.14`. Application v0.10.0
was published and its remote branch/tag verified at `7b23114`; collection v0.2.2
follows that delivered interface.

The inspected skill baseline said Python 3.10 or newer. Corrected only this
prerequisite and added an installation evaluation case. Names, routing, CLI/API
examples and authorization boundaries are unchanged. The clearml-yolo skill has
no conflicting Python version requirement and required no edit. No installed skill
copy was changed.

## Executed checks

- Shared architecture validator, planning and acceptance phases: exit 0; inspected
  no-impact record.
- Codex skill-creator `quick_validate.py skills/cveta2`: valid.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0; all existing
  skills including cveta2 and clearml-yolo were discovered without installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'`: exit 0, all 32 documents passed, including
  the final evidence and release prose.
- Packaged relative-link containment/existence check: passed.
- `git diff --check`: passed.

## Fresh-context evaluation

Native agent `/root/cveta_skill_eval`, requested GPT-6 Luna with low effort, read
the skill and usage reference to answer the installation question for Python
3.11, 3.12, 3.13 and 3.14, including selection of an older tag. It correctly allowed
3.12/3.13, excluded 3.11/3.14 and directed older-tag users to that tag's package
metadata. It performed no installation or live-service action. Runtime model
settings are not independently exposed. This is a native-agent content evaluation,
not an OpenCode/DeepSeek performance claim or an automatic routing evaluation.

Final source/package reconciliation inspected both changed reference files and
the task-owned plan, architecture/evidence and collection release metadata. No
executable module, dependency contract, invocation or effect boundary changed.
Collection v0.2.2 uses the existing Git-tag-only release process after application
delivery; no deployment or skill installation is part of this task.

## Delivery

Published collection v0.2.2 at `3d44ebc8d52db6de933145e34c0ed5176070eb61`.
The atomic main/tag push succeeded; remote branch and peeled annotated tag match
that commit. Tag object: `c846f14f380cee17b22b6bda6268099f4792eb81`. The
remote reports the repository moved to `custom-opencode-skills`; the existing
remote forwarded successfully and its local configuration was preserved.
