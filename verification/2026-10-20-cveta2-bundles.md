# cveta2 bundle guidance verification — 2026-10-20

Source initially inspected at collection commit
`740906249912671bb17f52edabacca247da27337`. Concurrent unrelated collection work
advanced through `0c9f5dd` and published v0.3.0; those changes were preserved.
The tool feature was developed from cveta2 commit
`39bc958d20cc21a85beeaf238e411d815c66e678` under feature 008.

Only the cveta2 skill entrypoint and usage/evaluation references were updated,
with adjacent task and release evidence. No installed package was modified.

## Source and packaging checks

- Actual verify/upload-dataset help and public Python signatures match guidance.
- A duplicate-key rejecting YAML check passed frontmatter, name/folder matching,
  supported metadata and description limits.
- All packaged relative links and anchors resolved.
- Skill-creator quick validation reported a valid skill.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` discovered all four
  collection skills, including cveta2's updated description; no installation.
- Full collection Markdown lint and `git diff --check` passed.
- Synthetic API/CLI examples produced report-only diagnostics without clean.csv,
  and clean.csv only after explicit cleanup; original source bytes stayed intact.

## Fresh native-agent evaluation

Requested host controls were gpt-6.1-sol, medium effort, fresh context. Actual
runtime controls were not independently observable. Frontmatter-only routing
selected cveta2 for malformed detection CSV inspection and reviewed ClearML
publication; it excluded model training/accuracy and repository development.

After explicitly reading the full skill and usage reference, the agent produced
appropriate proposed commands and evidence requirements for report-only checks,
explicit cleanup, complete preparation, missing empty-class choices, later upload,
edited payload/new parent, legacy CSV-only parent, known child failure, and removed
interfaces. It did not infer cleanup/default consent, rewrite review records,
retry an uncertain child blindly, or claim proposed remote actions were executed.
Both actual command-help checks passed.

This is native-agent routing and proposed-action evidence, not a controlled
before/after comparison or OpenCode/DeepSeek performance benchmark. No live
target-model run was performed. Synthetic artifact checks and the tool's live
integration tests are separate evidence.

## Independent review

Fresh read-only integrated reviewers requested as gpt-6.1-sol with high effort
examined tool behavior, public contracts and this companion guidance.
The first review found imprecise train/val wording: upstream requires nonempty
splits and a box in train; val may contain only backgrounds. The tool contract,
guide and this package were corrected together.

The subsequent fresh review returned **ship**, with no additional findings. It
independently passed 611 tool tests and all architecture gates. Parent tool
verification passed 2,760 tests with 93.25% coverage and 37 live integration tests.
Those tool results do not substitute for this package's structural checks.

Tool release: cveta2 v0.11.0, commit
`d68c8dfda1859025ceb443b047cefec844459a7d`. The tag was created through its native
semantic-release workflow; remote publication must be verified before this
collection is published. Final version labels and packaged anchors were checked
again after replacing the unreleased wording.

The companion release is prepared in a clean checkout that integrates the
current remote main and this task's source commit. This preserves unrelated
unstaged clearml-yolo guidance and local planning work in the authoring checkout.
Reinstallation remains outside scope.
