# Queue finalization and training GPU handoff guidance

Date: 2026-10-15. Collection baseline: `7ba08694f8829c77bcf5427f512d4486ce4f3b49`
on `main`, collection 0.2.3. Source worktree was clean before this documentation task.
Application baseline: `932614ba698dede3ec6405476846ecbbc95f0976`, with implementation
under its `queue-job-finalization` native bug assessment. The initial workstream marked the source release pending. The application release
and collection delivery checkpoint are recorded below.

Architecture: [no-impact record](../docs/plans/2026-10-15-queue-job-finalization/architecture.json).
No entrypoint metadata, executable modules, dependency contracts, scripts or installed
skills change in this collection. The task-owned scope consists of the usage/evaluation
references, architecture record, this evidence and collection release version/changelog.

## Inspected change and source reconciliation

The old usage guidance requires all GPU reservations through complete pipeline exit.
The amended user requirement separates queued jobs from GPU management and permits
other jobs to use extra GPUs during inference. The reference now describes full
training allocation, native training return and memory release before notification,
then guardian-owned durable contraction, extra-lock release, public allocation update
and acknowledgement. The first GPU remains reserved through owned descendant exit.
Absent a training-finished notification, full capacity stays reserved. Once a valid
notification has durably shrunk the claim, later private/public record-write failure
or a lost acknowledgement does not restore released GPUs. Failed acknowledgement
stops before inference; cleanup/recovery follows the remaining durable claim. It
does not establish successful pipeline completion.

Inspected the application's execution_lifecycle.py packet channel and bounded ack,
composition.py memory-cleanup/notification order, hydra/common.py supervised execution
boundary, and process_tree.py scoped live subreaper traversal. The source contract
requires guardian/lease implementation to complete durable shrink before freeing
extras, updating allocation and acknowledging; the application parent owns final
integration and executable verification of those changes.

The guidance distinguishes completed successful ClearML tasks from later guardian
cleanup uncertainty, which quarantines queue capacity as `recovery_required`.
Required computation/upload failures still fail. Inaccessible unrelated same-UID
processes are excluded from live scoped supervision; owner recovery retains
conservative global ownership proof. Neither artifacts alone nor a successful task
prove safe capacity reclamation. No recovery, release or shared-process action ran.

## Executed documentation checks

- Architecture validator `--phase plan` and `--phase acceptance`: exit 0. Scope is
  prose-only and reconciled with the collection's four task-owned paths. Acceptance
  structure does not establish behavioral evaluation or independent review.
- Codex skill-creator `quick_validate.py skills/clearml-yolo`: exit 0, valid skill.
- Duplicate-rejecting safe YAML parsing: metadata keys, description bounds,
  name/folder agreement and package-local entrypoint links pass. Metadata unchanged.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0; the four expected
  packages discovered without installation.
- `npx --yes markdownlint-cli2@0.23.3` over root, skills, verification and docs
  Markdown: exit 0, 35 files.
- Combined changed application/companion local-link check: exit 0, 66 local file
  and heading-fragment targets resolve.
- `git diff --check` in both repositories: exit 0.

The corresponding project-owned end-to-end skill and prerequisites were also updated
in the application checkout. Their skill metadata validator passes; its links remain
inside that repository. Seven changed application Markdown files pass markdownlint
with line-length checking disabled, matching the repository's existing long-line
prose. Initial default lint reported existing long-line issues and a double blank in
the touched project contract; the latter was removed. No application workload was
executed by this documentation workstream.

## Initial evaluation, review and release handoff

Two new evaluation cases cover the three-GPU handoff and the successful-task versus
uncertain-cleanup distinction. The parent will obtain fresh-context application of
both prompts and independent final review after integration. These are pending;
static source reconciliation is not a behavioral pass or automatic routing test.
No OpenCode executable is available here. Native Codex evaluation cannot establish
OpenCode/DeepSeek behavior. No GPU concurrency, ClearML publication or remote incident
reproduction is established by these documentation checks.

No commits, pushes, release tags or installation were performed by this bounded
workstream. Parent integration must reconcile final guardian source, record review
and evaluation outcomes, complete application release first, then replace the usage
release-pending label with its verified version and follow collection RELEASE.md.

## Parent behavioral evaluation, 2026-10-15

Two fresh-context native Codex agents applied the new prompts without reading the
evaluation answers. Both requested gpt-6.1-sol with medium effort; those are dispatch
settings, not independent runtime telemetry. The handoff answer checked source and
explained full training allocation, memory cleanup, durable guardian shrink before
acknowledgement, first-GPU retention, concurrent eligibility, and failure semantics.
It explicitly identified the pending-release qualification and did not submit work.
The cleanup answer requested actual version/status/queue evidence, rejected artifacts
as proof of success, separated completed task status from recovery_required capacity,
and did not delete claims or mutate tasks. Both satisfy the new evaluation criteria.

These checks do not establish automatic skill routing or target OpenCode/DeepSeek
execution. Application parent verification additionally passed the routine suite,
scoped real-lock/process regressions, direct native CPU and actual queued single-GPU
training/publication/download/reload, with owned-resource cleanup. Physical multi-GPU
contraction remains outside available hardware evidence. Fresh independent integrated
review and application/collection release delivery remain pending.

## Independent final review

Fresh native Codex `final_queue_review`, requested gpt-6.1-sol/high, reviewed the
integrated application and companion documentation after parent verification.
Verdict: **ship**, no blocking findings. The reviewer checked every task-owned
source/test/documentation path, architecture coverage and dated evidence, and
independently passed all 35 new application regressions. It found the companion
ownership, durable contraction and failure guidance consistent with implementation.
Runtime/model telemetry is not independently observable. Parent reconciled the
companion diff, including collection version/changelog release metadata, against the
no-impact scope. Release delivery remains pending until the application tag exists.

## Release checkpoint

Application source commit `427781f` and checked version commit
`a22157dc787d91b1324551892d311706bc8572c6` are published on `master`.
Annotated `v0.30.2` points to that version commit; its remote object
`ad2a496976fde7f3bf614e1f2797632b5ed12921` and peeled commit were verified.
Both mandatory commit/release suites passed. Tagged wheel and sdist fresh
application installs passed all 12 command helps and runtime/module probes;
release validation confirmed dependency metadata is unchanged.

The usage heading now names the verified application version. Collection 0.2.4
is a compatible guidance correction with updated VERSION and changelog. Parent
rechecked all 35 Markdown files, local links, strict metadata and no-impact
architecture acceptance. Six task-owned paths are reconciled; no installed skill
or executable code changed. Independent review and both behavioral evaluations
passed. This checkpoint is ready for the documented annotated-tag atomic delivery;
final collection refs are recorded in the application's queue-job-finalization
release evidence after publication. No deployment, package or asset publication
is authorized by this documentation release.
