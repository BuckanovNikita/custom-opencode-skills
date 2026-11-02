# cveta2 managed dataset usage guidance

Update the existing portable cveta2 package for strict personal/shared configuration, reviewed migration, mixed CVAT/S3 sources, opaque external transformations, stable 60/20/20 splits, CVAT feedback, cold start and three-file ClearML publication. Tool implementation and Spec Kit artifacts remain authoritative; this collection adds operating guidance, not a second implementation workflow.

## Ownership and compatibility

Only skills/cveta2/SKILL.md and its usage/evaluation references are authored package changes. Preserve pre-existing clearml-yolo edits and local branch divergence. Reconcile on a clean checkout of current remote history; preserve newer fetch diagnostics, timing, incomplete-issue and consolidated baseline guidance. No installed host copies, settings, plugins or secrets are changed.

## Architecture assessment

See architecture.json: inspected documentation-only impact, no executable modules, dependencies or services. No tool code is added to this repository.

## Verification and release

Validate frontmatter/local links, skill discovery, Markdown, example schema and actual implemented CLI/API signatures. Native fresh-context evaluation covers shared override refusal, arbitrary transformation/unmapped lineage, feedback retry and development near-miss routing. One fresh native context applies four cases; this is not OpenCode/DeepSeek runtime acceptance. Obtain fresh independent review of the reconciled package. Publish collection version 0.7.0 after the tool release using RELEASE.md Git-tag-only procedure in the clean release checkout; leave the original diverged user branch and unrelated edits intact. No skills installation or package assets.
