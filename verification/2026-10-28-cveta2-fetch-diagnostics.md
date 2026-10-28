# cveta2 fetch diagnostics guidance — 2026-10-28

## Scope and source

Collection source inspected at `44426a6` (VERSION 0.3.0). The companion tool is
cveta2 branch `feat/fetch-speed-diagnostics`, native feature
`specs/001-fetch-speed-diagnostics`, based on v0.11.1 commit `13ffec2`.
Implementation changes were still uncommitted during these checks; publication
status and final companion commits are parent delivery evidence, not inferred here.

Updated only [entrypoint](../skills/cveta2/SKILL.md),
[usage](../skills/cveta2/references/usage.md) and
[evaluation](../skills/cveta2/references/evaluation.md). Inspected source owns
CvatRequestDiagnostics/CvatApiError, required versus optional task reads, S3
failure categories, opt-in debug CSVs, inventory pruning and timing logs.
The initial edit replaced a retired dataset-contract link; reconciliation below preserves the newer upstream baseline anchor.
The [architecture record](2026-10-28-cveta2-fetch-diagnostics-architecture.json)
records documentation-only impact under the tool's native planning scope.

Existing unrelated clearml-yolo package edits and untracked planning/evidence were
preserved. No version/changelog, installed package, tool configuration, service,
credential, Git commit or tag was changed by this documentation workstream.

## Changes and checks

Guidance distinguishes actual failed request context from component attribution,
retained SDK statuses from unidentified transport errors, cveta2 attempts from
underlying HTTP attempts, and elapsed time from summed worker time. It calls out
incomplete optional issues despite strict mode, omitted raw SDK response dumps,
ordinary `.tasks` preservation and pruning from the complete inventory snapshot.
Five evaluation prompts cover gateway context, retry/timing interpretation,
optional issues, safe Python diagnostics and fetch/cache side effects.

Executed checks:

- `quick_validate.py skills/cveta2` from the available Codex skill-creator tooling:
  passed frontmatter and package checks.
- Additional YAML parsing rejected duplicate keys; checked name/folder agreement,
  supported metadata, field lengths and all three packaged local links: passed.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0; discovered
  cveta2 with its existing operations-only description. This was discovery,
  without installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`:
  exit 0 both before and after adding evidence (54 Markdown files in the final run).
- Ran the new documented `report_cvat_failure` Python function locally with
  synthetic typed diagnostics and absent diagnostics: passed. Verified public
  facade identity, old three-positional-argument construction and keyword-only
  diagnostic parameter against the implemented source. No remote requests.
- `git diff --check`: exit 0. Architecture no-impact validator: exit 0 for both
  planning and acceptance phases. These structural checks do not substitute for
  behavioral review.
- Existing command options and failure behavior were inspected against current
  cveta2 source and configuration documentation; no new CLI option was invented.

## Initial review and runtime limits

Fresh-context behavioral evaluation of the new prompts and final independent
review are coordinated by the parent after integration. They are not established
by discovery, lint, source inspection or the synthetic Python example. No live
OpenCode/DeepSeek/GLM/Qwen runtime or live CVAT/S3 execution was attempted by this
workstream, and no target-model performance claim is made. Tool-wide serial live
integration and release evidence remain in the native cveta2 feature record.

## Reconciled collection snapshot

The original checkout remained at `44426a6b6fce058ec9ab37588483eb4bc860ca0a`
(VERSION 0.3.0), with unrelated clearml-yolo changes. A new isolated clone uses
published collection `a9dea49470344d6fbfbc3a9e0381d169b1ac1a81`, VERSION 0.5.0,
including all intervening collection releases. Existing remotes and the original
working tree were not changed. The upstream dataset-contract anchor
`specs/baseline.md#dataset-format-and-fetch` is retained. Reconciliation changed
no other upstream package behavior.

At the reconciliation checkpoint, cveta2 source was still the uncommitted `feat/fetch-speed-diagnostics`
working tree based on `13ffec24a0b8fe631eabc8a22ca0542886c98498`, declared version
0.11.1. The guidance therefore retains its development-feature availability note
until the parent confirms the actual published application version and commit.
Collection release metadata remains pending; proposed minor version 0.6.0 follows
RELEASE.md's compatible-capability policy. No release is claimed at this checkpoint.

The final package adds one benchmark-overclaim prompt: fixture speed gains must
not become universal production promises or justify unrequested worker changes.
The usage reference explains comparable workers, cache state, workload and output
checks, alongside fixture limitations. Cache guidance now says the complete
inventory is captured before filters and after promotion; pruning itself occurs
later in the fetch flow.

### Prior recorded fresh-context usage review

The parent retained the prior `/root/usage_review` result: fresh gpt-6.1-sol,
medium effort, read-only, PASS with no blockers. Five package-informed scenarios
produced the expected observations:

| Scenario | Recorded observation |
| --- | --- |
| Exhausted 503 | Reported partial/default behavior and kept component attribution hypothetical. |
| Retry accounting | Distinguished cveta2 outer attempts from total HTTP requests. |
| Ordinary fetch debug files | Preserved an existing `.tasks` directory. |
| Optional issue 429 | Reported degraded issues even under strict fetch. |
| Benchmark claim | Rejected extrapolation of fixture results to every production fetch. |

This is retained prior review evidence, not a newly executed evaluation in the
reconciled clone. It is package-informed application evidence, not automatic
routing evaluation, live OpenCode/DeepSeek/GLM/Qwen execution or a measured
before/after assistant-runtime comparison. The prior review recommended an
explicit benchmark-overclaim prompt (now added) and a published-version
availability note (pending application release). A final independent review of
the reconciled release snapshot remains pending.

### Final review clarification

The parent final reviewer found an overly broad zero-S3-cache statement across
native documentation. Source inspection confirms that `_filter_cached` checks
the destination derived from the current frame/key, while basename fallback
must rediscover an unknown or stale canonical frame through missing-key GET and
listing. The alternate cached file is then reused without fetching its image
bytes again. Companion usage and evaluation expectations now state that limit;
no runtime persistence feature was added.

### Reconciled snapshot checks

- Codex skill-creator `quick_validate.py`: passed.
- Duplicate-key-rejecting YAML parser: metadata allowlist, name/folder agreement,
  supported field types/lengths and all three packaged local links passed.
- skills CLI 1.7.1 `add . --list`: passed; discovered all four expected skills,
  including cveta2. No installation was performed.
- The documented Python diagnostics function ran against the current tool source
  with synthetic diagnostics and absent diagnostics. Facade identity, the old
  three-positional-argument constructor and keyword-only `diagnostics` passed.
- Architecture validator passed both planning and acceptance for the no-impact
  record. Collection release metadata is included in its inspected scope.
- Initial reconciled Markdown run detected one extra blank line in this evidence;
  corrected before the final run. Final Markdown and diff results follow below.
- Final Markdown lint: passed for 64 files. `git diff --check`: passed.

## Release preparation and final review — 2026-10-29

The parent committed the feature as `bc03d40`, then merged it to application
`main` at `9159407`. Semantic-release selected cveta2 0.12.0; the companion
availability note is pinned to that introduced version. The collection version
is 0.6.0, a minor release for compatible usage capabilities, with changelog date
2026-10-29 (the local date when the release was prepared). The original
2026-10-28 evidence and review observations above remain historical checkpoints.

The independent `/root/acceptance_review` reviewed the reconciled companion
snapshot and returned **ship**, with no outstanding findings, after independently
running quick validation, architecture acceptance, Markdown lint for 64 files and
diff checks. This review includes the cache-rediscovery clarification and does
not change the external-runtime limitations recorded above. The final parent
version confirmation and this dated release record add provenance only.

Release preparation changes only the three cveta2 package files, this evidence,
the no-impact architecture record, VERSION and CHANGELOG.md. Inspection of the
complete task-owned diff confirms no module, executable import contract,
dependency or effect-owner changes. Original unrelated source work remains
preserved. Application release refs and companion publication are recorded below
after actual completion; they are not inferred from version selection.

### Confirmed application release and companion acceptance

The local cveta2 release is `v0.12.0` at
`a6b77f204790120650460f37dbacf32bd2e4ea6c`, feature
`bc03d408ed4685852a1280a6458af141d33a123d`, merged main
`9159407c2500ee2c079899ea0891e827baf5a8a4`. Release-commit hooks passed in
the native application workflow. Parent publication must precede companion
publication; local refs alone do not establish remote publication.

After pinning availability and finalizing VERSION/CHANGELOG.md, quick validation,
architecture acceptance, Markdown lint for 64 files and `git diff --check` passed
again. Remote collection main still resolves to the inspected base `a9dea49`;
there was no published `v0.6.0` tag at the final precommit check. No installed
skills, original working-tree edits, remotes, services or credentials changed.
The companion release uses one Conventional Commit and annotated `v0.6.0` Git tag;
its commit and verified remote refs are reported in the parent delivery receipt.
