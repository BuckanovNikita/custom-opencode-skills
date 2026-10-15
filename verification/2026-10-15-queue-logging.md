# Queue worker logging guidance verification

Date: 2026-10-15. Companion baseline: `e5c6512` on `main`, collection 0.2.2.
The source tree was clean before this task. The associated application work starts
from `634f3e2` on `fix/queue-default-logs-20261015`.

Architecture: [documentation-only record](../docs/plans/2026-10-15-queue-logging/architecture.json).
Task status is maintained in the [plan](../docs/plans/2026-10-15-queue-logging/plan.md).

## Baseline and planned scope

The existing portable package explains queue submission, ownership and GPU controls,
but contains no logging-level, status-heartbeat or child-log destination guidance.
This is an inspected reference gap, not a completed fresh-context behavioral test.

The parent-coordinated fresh native agent `usage_baseline` then answered: “Explain
how to tell whether my cy-queue worker is idle or waiting for GPUs, how to increase
diagnostics, and where pipeline logs are kept. Do not run or submit anything.”
The baseline package could not establish idle/waiting fields, a verbosity option or
the pipeline log path. The agent offered `list`/`gpu list` and correctly identified
the missing evidence. This observed retrieval gap justified the reference addition.

The application's existing guardian combines pipeline stdout/stderr in
`<submitted project root>/.cy/queue-logs/<JOB_ID>.log`, with an owner-private file.
It does not write that stream to a private snapshot `job.log`.

## Source reconciliation

Inspected the implemented worker parser and `_WorkerLog` in
`clearml_yolo/entrypoints/queue.py`, the application worker observation callback,
and the guardian's `_log_path`/child redirection. Worker verbosity choices are
DEBUG, INFO, WARNING and ERROR, defaulting to INFO on stderr. INFO includes startup,
changed owner-job details, an immediate and 60-second heartbeat, and clean shutdown.
DEBUG includes each poll's summary. Old terminal history is counted without replay.
Submission IDs and JSON remain on stdout; child pipeline output retains its existing
owner-private project log file. Inspected application source commit
`d817adda23cad20a1a62df732042314efeab8dc2` contains this implementation on top of
published compatibility release 0.30.0 at `2aa54fccbc1580ce92ab995209527d72977c1f91`.
The application was published first as
[clearml-yolo v0.30.1](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.30.1).
Annotated tag object `067ce7403c4f2501c72988fadadc959f9450aba3` resolves to release
commit `e4a7a735fd51cf038347e760bffd949f35f02908`. Both the parent and this
companion workstream verified the remote tag and master, which advanced to
`8a4932304caed060e5e0c4cc63eacb77c8220fca`
after a documentation-only merge of the concurrent compatibility delivery record.
Collection 0.2.3 follows that published interface.

The parent reports the application's native source and release checks each passed
2,511 tests with 30 skips and no failures/errors. Built-wheel help and a real idle
worker smoke test outside the checkout also passed. These are application release
results supplied by its workflow owner, separate from this collection's checks;
they do not establish training, GPU workload or ClearML publication success.

The entrypoint metadata and installed copies are unchanged. The complete companion
scope is prose guidance, evaluation cases, plan/architecture/evidence and release
metadata; no runtime, import, dependency or checking-tool changes are present.

## Executed checks

- Architecture planning and acceptance structural validators: exit 0. The no-impact
  classification was reconciled with all task-owned tracked and untracked paths.
- Codex skill-creator `quick_validate.py skills/clearml-yolo`: exit 0, valid skill.
- Duplicate-rejecting safe YAML parsing: valid allowed keys, string metadata,
  name/folder agreement and description bounds. All three packaged local references
  exist and remain inside the distributed package.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0; all four expected
  skills discovered without installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'`: exit 0, 34 documents checked.
- `PYTHONPATH="$PWD/src" cy-queue worker --help` from the application source checkout:
  exit 0; actual parser displays all four levels, default INFO and DEBUG poll behavior.
  Help exits before runtime initialization. No workload or submission was executed.
- `git diff --check`: exit 0.

## Fresh-context evaluation and independent review

The parent-coordinated native agent `/root/final_review` evaluated the updated
package with the exact worker-diagnostics prompt recorded above. Its answer correctly
covered default INFO on stderr, immediate/60-second idle or active state counts,
`waiting=` reasons, installed-help inspection and `worker --log-level DEBUG`, the
submitted project's `.cy/queue-logs/<JOB_ID>.log`, and unchanged JSON/IDs on stdout.
It respected the explanation-only boundary and did not start a worker or submit work.
This passes the changed reference's application case. The missing-option case was
added as a future evaluation prompt; no separate fresh-context run is claimed.

The reviewer returned “ship” for the final code and companion guidance with no
outstanding findings. Its read-only companion whitespace and no-impact architecture
acceptance checks also passed independently. The author inspected the task-owned
diff and kept the runtime/dependency-free classification. These are native Codex
agent results with inherited parent settings; resolved runtime model/effort were
not independently exposed. They establish content application and review, not
automatic skill routing or target-model performance.

## Collection release preparation

The collection 0.2.3 release files are prepared and the application's published
0.30.1 source is reconciled. A fresh remote fetch found collection `origin/main`
and local `HEAD` both at `e5c6512`, with no local/remote `v0.2.3` tag and no unrelated
working-tree changes. The seven task-owned paths are the two references, plan,
architecture record, this evidence, changelog and version file. Publish through
the documented atomic Git main/tag push, then verify the remote refs and record
delivery. No skill installation is part of this release.

No OpenCode executable is available in this environment. Native-agent evaluation
does not establish automatic routing or live OpenCode/DeepSeek behavior. No GPU run,
training, ClearML publication, skill installation or shared-service mutation is
claimed by these companion documentation checks.

## Delivery

Published collection `v0.2.3` at release commit
`a94cd2d0303117c3df2844f1a8c0fa73799bcb21` after the application's release.
The atomic main/tag push succeeded. Remote main and the peeled annotated tag both
matched that commit; tag object `4df86c609b14734297f84c69bc93d9d1c028bcf0`
was verified as an annotated Git tag. The worktree was clean after publication.
The remote reports its move to `custom-opencode-skills`; the existing SSH remote
forwarded successfully and its local configuration was preserved. This delivery
record completes the plan without changing the published tag or skill content.
