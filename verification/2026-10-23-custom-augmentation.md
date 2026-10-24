# Custom augmentation usage verification, 2026-10-23

This documentation companion describes clearml-yolo feature
`004-custom-augmentation`. The initial inspected CY checkout was `master`, version
`0.38.0`, at base commit `d8a3695eb481cdcf9fd523a17fee53a411bcf514`, with the
feature implementation present as uncommitted changes. The initial collection checkout was
`main`, version `0.3.0`, at base commit
`44426a6b6fce058ec9ab37588483eb4bc860ca0a`, with uncommitted companion changes.
These are historical inspected source identities, not augmentation release claims.
No skill installation was performed. The isolated delivery base is recorded below.

## Scope and preservation

The task adds [augmentation guidance](../skills/clearml-yolo/references/augmentation.md),
its routing paragraph in [SKILL.md](../skills/clearml-yolo/SKILL.md), a reference in
[usage.md](../skills/clearml-yolo/references/usage.md), and the two prompts under
[Custom training augmentation](../skills/clearml-yolo/references/evaluation.md#custom-training-augmentation).
The [architecture record](2026-10-23-custom-augmentation-architecture.json) classifies
this as documentation-only work. It changes no executable helper, dependency,
composition target or import contract.

Comparison against the pre-task copies of the three modified shared files shows
only those routing and evaluation additions belong to this task. Existing asset,
offline recovery, queue installation, Agent replay and ClearML cache changes remain
preserved and excluded. Existing portable-training verification and cache-planning
files are also outside this task's ownership. No complete-file staging of those
three shared files in the original checkout is appropriate while its unrelated
edits remain uncommitted. The isolated delivery checkout contains only this
task's changes relative to the current upstream base.

## Executed checks

| Check | Observed result |
| --- | --- |
| Skills CLI 1.7.1 `add . --list` | Earlier retained output lists all four collection skills, including `clearml-yolo`; discovery only, no installation. |
| Skill-creator `quick_validate.py` | Repeated for the current package; passed. |
| Explicit YAML and package validation | Valid name/folder agreement and description bounds; unknown metadata fields rejected by the check; injected duplicate YAML keys rejected. All seven local link references resolve inside the package. |
| Current CY source inspection | Selector names, disabled native controls, conflict warnings, frozen snapshot replacement, supported bbox formats, first-top-level Mosaic placement, donor policy and prohibited transforms match the described interface. |
| Architecture plan and acceptance validation | Both phases passed for the no-impact record. Structural validation does not establish native execution. |
| Markdownlint CLI2 0.23.3 | The original-checkout evidence-inclusive run checked 53 Markdown files with zero issues; isolated delivery checks are recorded below. |
| `git diff --check` | Passed after the task-owned documentation additions. |

Source inspection covered `entrypoints/hydra/transforms.py` and the shared
`adapters/augmentation/serialization.py` and `pipeline.py` in CY. The parent also
validated the documented examples against the source interface as part of the
CY documentation stage. Native training, GPU and ClearML publication acceptance
belong to the CY feature evidence; this skill check makes no such claim.

## Behavior evidence and limitations

Two fresh native-agent contexts explicitly read the skill and its augmentation
reference. The cases tested application of supplied guidance, not automatic skill
routing. The current evaluator did not rerun those agent sessions.

For the RF-DETR nano prompt asking to retain native color jitter and multiscale,
the parent reports a passing response: it used the file-backed transform selector,
explained replacement of competing native randomness and retained final sizing and
normalization. The full response was not retained in the resumed evidence, so this
is parent-reported case evidence, not independently re-inspected output.

The Faster R-CNN Mosaic response is available in the inherited evaluation record.
It asked for the existing JSON to preserve its contents, placed one `A.Mosaic`
first in the top-level `Compose`, rejected validation/test donors and `Normalize`,
and retained backend final preprocessing. It explained raw training donor sampling
and the frozen JSON/digest snapshot, including file-independent replay only after
freezing succeeds. It did not run a workload, invent inline JSON input or claim
identical stochastic results. That response satisfies the supplied case.

The parent reports that a fresh independent reviewer inspected the task-owned
skill content and found no additional blocker. The current evaluator inspected
source consistency and packaging, rather than treating that prior verdict as a
new independent review. Requested or realized evaluator model settings are not
established by this record. No live OpenCode/DeepSeek, GLM or Qwen evaluation was
performed; static checks and native-agent reading do not establish those runtimes'
performance.

## Isolated delivery reconciliation, 2026-10-24

The initial collection checkout was behind upstream. The inspected `origin/main`
delivery base is `593f4e9760e555d4449d5067ae77a53ce443ec96`, with `VERSION` equal
to `0.4.3`. Its history already includes released portable-training and cache
guidance. Those additions are part of the delivery base and are not included in
this task's patch.

A detached worktree was created at that exact commit. Only the augmentation routing
paragraph, usage link, evaluation section and three task-owned new files were
transferred. Existing upstream guidance, including changes absent from the stale
local checkout, was retained. The original checkout's files and index remain
unchanged. No branch, version or changelog change was made.

The clean baseline Markdown check passed for 60 files. Independent checks after
transfer passed in the delivery worktree:

- Skills CLI 1.7.1 discovery lists all four expected skills without installation.
- The skill-creator validator passes; explicit metadata validation rejects duplicate
  YAML keys and verifies name, description and supported metadata constraints.
- All nine packaged local link references, including anchors, resolve inside the
  package. The increase from the original checkout reflects retained upstream links.
- Markdownlint CLI2 0.23.3 checks 62 files with zero issues.
- Both plan and acceptance phases of the no-impact architecture record pass.
- `git diff --check` passes. The three modified shared files differ from the exact
  upstream base only by the intended 26 added lines; the three new files are task-owned.
- Before/after hashes of the original checkout's 71 tracked and untracked files,
  its index hash and its porcelain status are unchanged.

This transfer is preparation only. CY native acceptance, fresh final review and
both repository releases remain separate pending gates.

## Delivery status

The source reference remains explicitly marked under implementation. CY native
acceptance and the implementation release are pending. After the implementation
is accepted and released, update the reference with the actual released CY
version, reconcile this record with final evidence, and complete the collection's
[Git-only release workflow](../RELEASE.md). Stage only the six task-owned files in the isolated delivery checkout,
preserving the original checkout and all unrelated changes above. No companion commit, version tag,
push, or installed-skill export has been performed for this task at this checkpoint.

## Fresh integrated source review, 2026-10-24

The fresh read-only native reviewer `/root/final_integration_review` inspected this
six-file companion patch against `593f4e9`, together with the integrated CY feature
and final local verification. Its code-review verdict was ship, with no blocking
correctness, architecture, preservation or documentation findings. It independently
ran `git diff --check` in both worktrees. It did not run native workloads or claim
the gated all-model matrix complete. The CY full local suite at that checkpoint
passed 3,226 tests; 30 were skipped and 47 opt-in cases deselected. Native matrix,
cleanup, architecture acceptance and both Git-only deliveries remain pending.

## Authorized release preparation, 2026-10-24

After disclosure that 39 of 45 required pretrained Severstal scenarios passed and
six DEIMv2-L/X scenarios still needed authorized DINOv3 Small/Small+ weights, the
user requested "make a release". The parent treats this as a scoped instruction to
publish with those native checks unresolved. It does not change the recorded
native result or turn the incomplete CY architecture acceptance gate into a pass.

The final CY native evidence records optimizer updates, downloaded artifacts,
checkpoint reload and matched initialization for the 39 successful cases. Six
separate random-initialization probes failed in the upstream box matcher before
their first optimizer update and do not establish pretrained acceptance. The parent
archived and verified the retained evidence before cleaning all owned native runs.
No full-dataset training, physical multi-GPU acceptance or skill installation is
claimed.

A fresh isolated clone on `main` starts from current upstream commit
`593f4e9760e555d4449d5067ae77a53ce443ec96`. It carries the reviewed six-file
augmentation companion plus `VERSION` and `CHANGELOG.md`. Collection candidate
`0.4.4` is a patch update to existing usage instructions; it adds no skill,
executable capability or incompatible interface. Both earlier source checkouts
remain unchanged. The documentation-only architecture scope includes release
metadata and still has no runtime or dependency effect.

The parent must supply the actual published CY version and commit before the
augmentation reference is finalized and this collection is committed or tagged.
This preparation record does not claim either repository has been released.

Preparation checks passed: Skills CLI 1.7.1 discovered all four skills without
installation; the skill-creator validator and explicit metadata checks accepted
the package, including a rejected duplicate-key fixture. All nine package-local
references resolve within the package. Markdownlint CLI2 0.23.3 checked 62 files
with no issues, and the CY documentation checker checked 61 maintained Markdown
files and local anchors with no issues. Both no-impact architecture phases and
`git diff --check` passed. These are documentation/release checks, not new native
training or target-model behavior evidence.

## Published application and companion finalization, 2026-10-24

The parent published and verified clearml-yolo
[v0.39.0](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.39.0): release
commit `6cae69e7c31204f1b2266ec064075711426bb6c4` and annotated tag object
`952237d1dd27caa78eae6526163fd0e9ed76bc47`. The remote `master` and peeled tag
both resolved to that release commit. The augmentation reference now identifies
this published implementation instead of the earlier development checkout.

Both the ordinary contributor-commit hooks and automatic version-commit hooks
passed, including their full pytest runs. An earlier full-suite attempt terminated
with SIGILL; the isolated asset module, asset selection after full collection and
exact timeout case passed on investigation. The unchanged normal hook retry then
passed. No source workaround was introduced, and the interpreter crash's cause
remains unconfirmed. The application
[release evidence](https://github.com/BuckanovNikita/clearml-yolo/blob/v0.39.0/specs/004-custom-augmentation/evidence/release.md)
retains the failed observation and bounded investigation.

The parent reports that the fresh independent release audit returned **ship**.
That audit and the successful hooks establish release-check evidence, not complete
native acceptance. The six gated DEIMv2-L/X pretrained cases remain unverified,
with the supplemental failures retained separately. The user's explicit release
instruction applies to that disclosed gap; the incomplete CY architecture/native
gate is not relabeled as passing.

After adding the published-version links, skill discovery, metadata validation,
both documentation checkers, both no-impact architecture phases and whitespace
checks passed again. The version-pinned evidence paths exist in the actual
`v0.39.0` application tag.

Collection candidate `0.4.4` contains only the eight documented paths. Its source
is ready for parent inspection and the collection's Git-only commit/tag workflow;
no companion commit, tag or push has been performed at this checkpoint. No skill
installation, package publication or GitHub Release object is included.
