# clearml-yolo usage guidance release: 2026-10-15

Collection target: `0.2.1`, on `main`, based on
`1e0ed14ff1346eb168208118e089635e0f575351`. This patch release adds compatible
usage documentation for implemented tool interfaces; it adds no runtime module.
The [architecture record](2026-10-14-clearml-yolo-gpu-controls-architecture.json)
records the inspected no-impact decision and the complete task-owned scope.
The [earlier evidence](2026-10-14-clearml-yolo-gpu-controls.md) retains the
prepublication state and behavioral application observations.

## Source reconciliation

Inspected the implemented CLI parser, GPU control adapter, queue guide and DDP
verification guide. The examples preserve the distinct physical operator versus
CUDA-logical submission selectors, owner holds, current claim tokens, whole-claim
release, mandatory reason, force refusal and no-signal behavior, compatibility
fencing and supervision capability preflight. No executable interface, import
contract, dependency or checking tool changes in this companion collection.

Tool implementation was published first as
[clearml-yolo v0.29.0](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.29.0).
The annotated tag object `c8c4257ac43eed59ad7aa7c8280af49c49568abb` resolves to
`28db0665127e966e4dfe802d891a7c4df14cdce4`, matching `origin/master`. The
implementation commit is `c450411b6ba7dc0cb4fbde2259cd79f43ffdd2d7`. Local
tag/ref resolution was checked directly; the primary agent verified the published
remote refs, ordinary source/release hooks, full pytest and immutable wheel/sdist
checks, including isolated extracted-wheel CLI validation. Those checks establish
release provenance and offline behavior, not physical GPU acceptance.

## Executed checks

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0;
  all four expected skills discovered. List mode performs no skill installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`:
  exit 0 on the final 30-file scope, including this evidence.
- Duplicate-rejecting safe YAML parser: valid frontmatter, allowed metadata,
  name/folder agreement and description bounds; all six package/evidence local
  links pass, including the three bundled package references.
- Actual `cy-queue gpu --help` and `cy-queue gpu release --help`: exit 0;
  selector, token, reason and force arguments match the documented examples.
- Architecture record plan and acceptance structural validation: exit 0.
  Reconciled the complete task-owned diff, including all three untracked evidence
  files, against the no-impact scope; no executable module changes are present.
- `git diff --check`: exit 0 on the prepared collection version/changelog changes.

## Review and limitations

The independent `release_review` accepted all eight final task-owned paths after
provenance edits, with no remaining findings, and independently verified the tool's
published refs. The primary agent inspected the final diff and owns collection
publication. No GPU operation, queue submission, skill installation or shared-service
mutation was performed.
The target physical multi-GPU/NCCL/ClearML acceptance awaits user-returned results;
this release does not establish it. Prior native skill-application checks are
reported in the earlier evidence; no new automatic-routing or live
OpenCode/DeepSeek execution is claimed.
