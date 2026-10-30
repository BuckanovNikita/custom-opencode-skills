# CY weight Files remote recovery — 2026-10-30

## Scope and state

The collection candidate starts at fetched `origin/main`
`9da53d6dd7b5e89538f5c232541f2faa95b1f72a` (0.6.3). Queue source is fetched
`origin/main` `397753c123f379ade50f06a2babf5e4b1476b9d2` (0.1.5), with the
existing compatible `clearml-yolo>=0.38,<0.45` requirement. CY starts at
`2c494c65cea32cfaccd91a2607333d764b1691ec` (0.44.0); its proposed patch is 0.44.1.

The user's layout is a GHR Files remote of `https://github.com` and a GH VCS remote
of the same upstream. Existing URL construction already matches that split.
The companion fix addresses archive diagnostics lost before final reports,
archive time limits, and identical ResNet weights rejected solely after a supported
Torchvision version changes. The user's subsequent correction makes bulk preparation
release-only: valid native caches are reused first, and a missing asset must come from
the project's pinned release. Original-host requests remain available only to the
separate standalone model resolver. This collection documents that distinction rather than
claiming that original-host TLS/connection errors establish an Artifactory root cause.

[Architecture record](2026-10-30-weight-files-remote-architecture.json) records the
seven-path documentation-only scope. No executable helper or package dependency is
changed. Original shared checkouts and installed skill copies remain untouched.

## Acceptance plan

Update the portable entrypoint, model-assets usage and evaluation prompts; validate
Markdown, local links, unique YAML/frontmatter, discovery and Bash syntax against
integrated CY source. Record actual checks below. Fresh behavioral evaluation and
independent review are parent-owned. The proposed CY behavior stays explicitly
unreleased until its actual release and installed metadata have been verified.

No live Artifactory requests, training, GPU workloads, uploads or skill installation
are authorized by this documentation task. Fixture/static evidence is not a server
readiness or live target-model execution claim.

## Documentation checks

The collection candidate passed discovery (`skills@1.7.1 add . --list`) for all four
packages without installing them; the CLI's generic noninteractive banner did not
change the requested list-only operation. `quick_validate.py` accepted clearml-yolo.
Strict YAML/schema checks accepted all four package metadata records, rejected a
duplicate-key negative control, and found no package symlinks. All 19 Bash fences
in the three affected skill files passed `bash -n`; none were executed.

CY's `scripts/check_docs.py --root <collection> --all` plus this new evidence file
passed with 68 governed Markdown files and no local-link/anchor issues. The first
Markdown lint found an extra blank line and a bare GitHub URL in the new evaluation
prompt; both were corrected before the rerun. Architecture plan/acceptance structural
validation and `git diff --check` passed. Runtime behavior and fresh evaluation remain
pending parent integration and review; these documentation results do not establish
live Artifactory connectivity or target-model performance.

The user subsequently requested a new visible weights release. The parent plans
`weights-2026-10-30` with the existing verified model payloads, a new manifest pin,
and visible latest-release metadata. This is a specifically requested model snapshot;
it does not change the independent release cadence or authorize routine code-release
asset uploads. Publication is pending and must be verified separately.

The corrected Markdown lint rerun passed all 69 files with zero issues. The two new
evaluation cases cover original-host error interpretation/Files timeouts and unchanged
ResNet content across supported Torchvision versions. Their expected outputs were
reconciled with the user's release-only correction; fresh session execution is pending.

## Integrated source and proposed pair

The parent signaled integrated runtime readiness. An isolated CY source snapshot
normalized only the project/root-lock version from 0.44.0 to proposed 0.44.1; the
copied manifest pins `weights-2026-10-30`. The current source and changed examples
agree on release-only bulk resolution with `local_only=True`, Files-root routing,
standalone GH/HF separation, archive default 1800 and ordinary default 300 seconds,
positive finite timeout validation, and the 3600-second shell example. The actual
installed `cy-download-weights --help` exposes only help, with no model-enabling flag.
Those checks made no download requests.

Normal editable resolution of the proposed CY, unchanged queue 0.1.5 and approved
external source packages passed. Installed/source versions, requirements, editable
origins and package imports agreed; `uv pip check` accepted 285 installed packages.
Installations used copy linking in the isolated pair environment, leaving the CY
canonical verification environment untouched. Queue Ruff, formatting, strict mypy
and both import contracts passed; integration completion remains pending below.

The proposed-pair integration suite completed with 518 passed, 7 physical opt-in
cases deselected and 19 upstream deprecation warnings in 125.15 seconds. Queue
CLI help passed; source/installed metadata still agreed afterward. No source runtime,
test or script drift occurred during the snapshot check, and the queue checkout
remained clean. No queue code or dependency bound change was needed. Actual released
CY metadata/source installation must still be refreshed after its release hook; this
proposed-pair result does not claim final release completion.

## Fresh evaluation and published model snapshot

A fresh read-only native usage evaluator returned `SHIP` with no findings for both
new cases. It correctly separated release-only bulk preparation from standalone
upstream resolution; used the GHR Files root and separate GH VCS route; preserved
version guards, native-cache reuse, explicit retry, timeout bounds and privacy; and
made no live Artifactory request or unsupported root-cause claim. This is native-agent
behavioral evidence, not a DeepSeek/OpenCode target-model run.

The parent published the specifically requested
[weights-2026-10-30 snapshot](https://github.com/BuckanovNikita/clearml-yolo/releases/tag/weights-2026-10-30)
as the visible latest GitHub Release (release ID `428749753`). Its six asset sizes
and server-side SHA-256 digests matched the prepared four tar archives, new manifest
and checksums. Model payload bytes are unchanged from the earlier verified snapshot.
A fresh hosted-download repetition is separately pending; metadata agreement alone
is not claimed as that repetition's result. Routine code releases still do not
republish model assets.

Parent CY verification completed with a genuine canonical receipt: static/import
checks passed and pytest reported 4,460 passed, 30 skipped and 81 deselected.
The documentation gate passed. Fresh code review and actual CY 0.44.1 release
metadata remain pending; the portable guidance retains its proposed-version guards
until the parent verifies that release. No installed skill copy has been upgraded.

## Timeout contract clarification and hosted restoration

Fresh code review found that the earlier timeout description overstated the
implementation. Requests checks CY's elapsed-download budget only after a completed
streamed chunk, while connection and read-idle timeouts remain separate at 10 and
30 seconds. Headers or continuous slow arrival can exceed the elapsed budget before
CY checks it; the archive lock's finite `4 * budget + 60` wait is not guaranteed to
cover every transfer/retry. The parent explicitly revised this contract rather than
adding an unrelated hard-deadline transport implementation. The portable entrypoint,
usage and evaluation expectation now describe those limits. The documentation
correction is applied; fresh review acceptance of this amendment remains pending.
The prior static example check established constants and interfaces, not a hard
wall-clock bound, and no such bound is claimed as verified.

The parent also completed a fresh hosted restoration from the published new release:
56 assets, exactly four archive GETs and zero original-host requests. Import-time SDK
probes were separately denied, rather than counted as successful bulk downloads.
This verifies the hosted project assets and bulk network boundary; it is not a live
Artifactory check. Actual CY release metadata remains pending.

## Actual checked CY release and final paired acceptance

CY's native hooks created release commit
`ad7204c10baf00abafe23b92bb8882e2fea22171` and annotated local tag `v0.44.1`,
following fix commit `254e988`. The parent inspected the generated change: only
CHANGELOG.md and the project/root-lock version changed. Final canonical verification
passed with 4,460 tests, and documentation/architecture acceptance passed. An
intermittent native SIGILL observed during verification remains explicitly recorded
by the parent; the unchanged full retry passed. This is not evidence that the
underlying native instability has been eliminated.

The final actual-source paired runner required the exact release HEAD/tag, then
normally installed that source with queue 0.1.5 and approved externals into the
isolated pair environment using copy linking. Dependency consistency, source/installed
versions and requirements, actual editable origins and imports all passed before
and after integration. Queue Ruff, formatting, strict mypy, both import contracts
and CLI help passed. The actual-source queue suite passed with 518 tests, 7 physical
opt-in cases deselected and 19 upstream deprecation warnings in 121.78 seconds.
Queue remained at clean commit `397753c123f379ade50f06a2babf5e4b1476b9d2` with no
code or dependency-bound change. The canonical CY environment was untouched.

The fresh Terra reviewer explicitly rechecked the timeout wording and portable
guidance after the contract amendment and returned `SHIP`, conditional on the final
canonical/docs/architecture/actual-pair gates now completed. The later native
instability review accepted the clean retry with its unresolved limitation recorded.
The earlier fresh usage evaluation covered both new prompts; these are native-agent
review/evaluation results and do not establish DeepSeek/OpenCode performance.

The skill now names the implemented CY 0.44.1 interface while retaining installed
version/source and actual release-evidence checks. Earlier pending entries above
preserve preparation history. Remote CY delivery and collection publication remain
parent-coordinated; the collection is not published by this verification step, and
installed skill copies remain unchanged.

## Verified remote CY delivery

CY was pushed atomically and the remote refs were read back: `master` is
`3636404eafaf0c32312fddb4269b8aad0e2b12eb`; annotated tag `v0.44.1` has object
`900b872046bdb84f92fdd6c51e38427b21bffde0` and peels to release commit
`ad7204c10baf00abafe23b92bb8882e2fea22171`. The final branch commit records release
verification after the version tag; runtime and package inputs remain unchanged
from the actual paired acceptance. The published
[CY 0.44.1 source](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.44.1)
and [latest model snapshot](https://github.com/BuckanovNikita/clearml-yolo/releases/tag/weights-2026-10-30)
now support the portable version guards and examples.

The parent reviewed the final portable diff and authorized collection 0.6.4 delivery.
Final collection checks passed: four-package discovery without installation,
69-file Markdown lint, 68-file local-link/anchor validation, skill validation,
unchanged strictly checked frontmatter, 19 Bash syntax checks without execution,
no-impact architecture acceptance and whitespace. The seven owned paths match the
architecture inventory. Collection publication uses its Conventional Commit and
annotated-tag workflow; no GitHub Release, asset upload, registry publication or
installed-skill update is part of this collection delivery.
