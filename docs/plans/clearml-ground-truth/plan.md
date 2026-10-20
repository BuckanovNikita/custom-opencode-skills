# ClearML ground-truth inputs and default dependencies

Update the portable clearml-yolo usage skill alongside the application's
[feature contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/001-clearml-ground-truth/contracts/input.md).
The application repository owns implementation and runtime acceptance.

## Architecture and ownership

The [architecture record](architecture.json) classifies this source update as
documentation-only. Preserve existing stable-cache additions in the usage and
evaluation references and the separate stable-cache plan. No installed skill,
runtime module, dependency configuration or executable check changes here.

## Tasks and acceptance

1. Update input selection and installation guidance using implementation evidence.
2. Add explanation-only success and failure evaluation cases for dataset selectors,
   queue snapshots, image resolution and default dependencies.
3. Validate frontmatter, packaged links, CLI discovery, Markdown and diff whitespace.
4. Record checks, application source identity and limitations in dated evidence;
   obtain independent evaluation/review after integration. Follow collection
   release policy after the application interface is published; installation
   remains outside this source update.

## Isolated integration — 2026-10-20

Prepare collection `0.2.11` from published `origin/main` commit
`351fde30139095668d40c7ee917b6276704bdc5e` (`0.2.10`) in a clean isolated clone.
Carry over only the three task-owned skill changes and this task's records;
retain published stable-cache and early-publication content. Include `VERSION`
and `CHANGELOG.md` in the documentation-only architecture scope. Revalidate the
combined collection before fresh integrated review. Commit, tag and push wait
for application publication and the parent workflow's release handoff.

## Application baseline integration — 2026-10-20

The application merged upstream stage-zero reset `0340e9c` after task commits
`a3f4c5a` and `de726d9`. Its feature records now use
`specs/001-clearml-ground-truth`; preserve earlier source/check history and use
that current path for navigation. The checked/built `aaa35d9` / `v0.36.0`
candidate was never pushed and is superseded. The final application commit and
release remain pending renewed integration acceptance. Companion publication
continues to depend on that application delivery; this reference update changes
no skill behavior or release metadata.

## Concurrent collection baseline integration — 2026-10-20

Published collection commit `e0ff164696d86eaca42e218623f6c9f03798f0ad`
(`0.2.11`) now owns the stage-zero source-reference corrections. Prepare this
feature as pending `0.2.12` in a new clean clone of that commit. Preserve the
published source-reference bullets, release history and cleanup-baseline records;
carry the accepted three-file skill behavior and task evidence without changes.
The earlier isolated `0.2.11` feature candidate remains unpublished and is
superseded for delivery. Validate the collection, package and current source
links again; this link/metadata reconciliation adds no behavioral change requiring
a new evaluation round. Final application commit/publication remains pending,
and collection commit, tag, push and installation are outside this handoff.

## Delivery handoff

Application `v0.36.0` was published at `42ce196bb4b91d8d4e80fa59397b730bb0d10cd2`
after required full hooks and final-source artifact checks. This satisfies the
application dependency for collection `0.2.12`; final collection validation and
Git-only delivery follow the repository release procedure. The dated verification
record preserves the earlier candidates, reviews, evaluations and limitations.
