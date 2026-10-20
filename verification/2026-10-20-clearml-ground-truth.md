# ClearML ground-truth skill documentation — 2026-10-20

## Scope and source

Application source: `clearml-yolo`, branch `feat/clearml-ground-truth`, inspected
working implementation based on commit `129b94b749d7d4bcaf6e82b9a504a141796c7bdc`
(package version `0.34.1` before feature release). The companion
[source contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/001-clearml-ground-truth/contracts/input.md)
belongs to the application feature. These examples describe that working feature;
its final commit, runtime acceptance and publication must be confirmed before
claiming the interface is released.

Task-owned source changes:

- `skills/clearml-yolo/SKILL.md`: source-input routing and default-installation rule.
- `skills/clearml-yolo/references/usage.md`: selector syntax, validation, CSV/media
  behavior, queue/replay provenance, local compatibility and dependency guidance.
- `skills/clearml-yolo/references/evaluation.md`: four explanation-only cases covering
  selectors, invalid inputs/media, queue/replay and default runtime installation.

The [plan](../docs/plans/clearml-ground-truth/plan.md) and
[architecture record](../docs/plans/clearml-ground-truth/architecture.json) classify
the collection update as documentation-only. Existing stable-cache additions in
the same usage/evaluation files and the separate stable-cache plan were preserved
and excluded from this task's ownership.

## Executed checks

- Skill Creator `quick_validate.py` accepted the changed package.
- A strict YAML loader rejected duplicate keys; name/folder agreement, name format,
  description length and portable field allowlist checks passed.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` listed all four collection
  skills including `clearml-yolo`. This was discovery only, with no installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'` passed.
- The application's read-only documentation checker validated the three changed
  skill files with the package as root: local links remained inside the package
  and resolved without issues.
- Both architecture validator phases passed for the documentation-only record.
- `git diff --check` passed in both repositories.
- In the application source environment, all three documented selectors composed
  and normalized for seven consuming Hydra configurations: 21 checks, no workload
  or remote dataset lookup invoked.
- Application Markdown/local-link validation passed for the updated README,
  technical reference, input/installation/queue guides, current contract index and
  project-owned end-to-end prerequisites. The latter now covers dataset retrieval
  acceptance and default dependencies.

## Limits and handoff

Source inspection and Hydra composition establish the documented interface shape;
they do not establish real ClearML retrieval, GPU work, publication, default wheel
installation or minimum-version runtime compatibility. Those remain application
feature acceptance responsibilities. No exact dependency floors were copied into
the skill: application metadata remains authoritative.

Fresh-context native Codex behavioral evaluation passed all four new cases: selector
configuration, malformed selectors/media, queued execution/replay/local compatibility,
and default dependencies. The evaluator inspected the skill and matching source and
returned simulated answers without executing workloads or installing software. One
precision issue was corrected: runtime-only synchronization uses `--no-dev`, since
ordinary uv sync includes its default development group. Exact agent model/effort
metadata was not observable. Integrated independent review remains pending.
No OpenCode/DeepSeek execution was performed or inferred from structural checks.
No source commit, collection version bump, tag, push or installation was performed
in the initial documentation pass. Follow collection maintenance/release requirements
after the application implementation is published, preserving unrelated changes.

## Isolated collection integration

Prepared an isolated clean clone from published collection commit
`351fde30139095668d40c7ee917b6276704bdc5e` (`0.2.10`). Applied only the three skill-file
changes from this task, including the corrected runtime-only `--no-dev` recipe,
and copied the current task plan, architecture record and evidence. The original
source checkout and its unrelated unstaged stable-cache changes were untouched.

The entrypoint and usage patch applied directly. The evaluation insertion was
placed before its original `Missing input` section after preserving the intervening
published stable-cache and early-publication cases. No published skill content was
removed or replaced. Prepared `VERSION` and a dated changelog section for `0.2.11`;
these are pending release metadata, not a published tag.

Reran collection validation against this integrated state:

- CLI discovery listed all four skills without installation.
- All four skill frontmatters passed unique-key, naming and portable-field checks;
  the changed package also passed Skill Creator validation.
- Full collection Markdown lint checked 46 files with no issues. Repository
  Markdown/local-link validation checked 45 files with no issues; package-root
  validation separately checked all three changed skill documents.
- Documentation-only architecture acceptance and `git diff --check` passed.
- `VERSION` and the dated changelog agree on `0.2.11`; neither the local repository
  nor the remote had a `v0.2.11` tag at inspection time.

Independent review, application publication and collection commit/tag/push remain
parent integration gates. No installation was requested or performed.

## Review correction: CSV-only inventory and SDK transport

Inspected the adapter's new pre-download guard: the current logical inventory
must equal `{dataset.csv}`. Updated both source copies of the portable skill to
require only root `dataset.csv` and reject additional files or linked entries.
The selected/materialized input remains the unchanged CSV; SDK archive chunks
and cache can still include historically removed media. Guidance now calls for
a fresh CSV-only dataset lineage when transport must exclude that media too.

The application README, technical reference, input/provenance contract and native
prerequisites were aligned. The failure evaluation case now checks extra files,
linked images and the limits of deleting media from an inherited version. Earlier
fresh-context evaluation preceded this tightened case; integrated review and any
fresh behavioral re-evaluation remain parent-owned acceptance work.

Revalidation after this correction passed: application Markdown/local links
(534 files), original collection Markdown lint/local links (44/42 files), isolated
collection Markdown lint/local links (46/45 files), both package-root link checks,
strict skill metadata, Skill Creator validation, architecture acceptance and diff
whitespace. These checks do not establish archive transport or live SDK behavior.

## Final independent acceptance

A second fresh-context reviewer returned **ship** for the final application and
companion documentation, with no remaining actionable findings. It verified the
CSV-only inventory guard, logical-input versus SDK archive distinction, selector
and replay semantics, dependency ownership and preservation of published guidance.
Application full-suite completion and final-source wheel rebuilding remain separate
release gates and are not inferred from this documentation review.

A fresh-context behavioral simulation reran the strengthened mixed-selector,
linked-image and inherited-dataset case using the final skill. It passed all
criteria: reject mixed selectors and extra entries, require finalized root CSV,
retain downloaded-CSV-relative media rules, add no media-remapping feature, and
explain the historical archive caveat and fresh-lineage requirement. No workload,
installation, service call or file mutation was performed by the evaluator.
This remains native Codex simulation, not a live OpenCode/DeepSeek model run.

## Application stage-zero integration: final commit pending

The application task commits `a3f4c5a` and `de726d9` preserve the earlier
implementation and evidence. The checked/built candidate `aaa35d9` with local
tag `v0.36.0` was not pushed: the ancestry gate detected upstream
`0340e9c503f495b6515592d1aa1452d4978e21ef` before publication. It is superseded
for delivery; prior acceptance above remains evidence for its original snapshot.

The application retains upstream cleanup and stage-zero contracts, and moves this
feature to `specs/001-clearml-ground-truth`. Updated the plan and current source
link to that path while preserving all dated checks and verdicts. Skill behavior,
VERSION and changelog were not changed by this reference reconciliation.
**Final application commit and published release: pending.** Renewed application
integration/review, final package checks and delivery precede companion publication.

The three inherited usage-reference links to retired native-configuration,
stable-cache and early-publication specs now target their stage-zero sections.
The combined configuration/publication reference names both relevant sections.
Read the actual baseline section contents and retained the accepted skill
instructions and evaluation behavior; this is a link-only correction.

Revalidation passed after this reconciliation: collection Markdown lint and
local-link validation (45 Markdown files), package-root documentation validation
(three files), Skill Creator validation, documentation-only architecture acceptance
and diff whitespace. Mapped all 11 unique current application source paths and
anchors in the changed plan, evidence and usage references to the merged application
checkout; every path and anchor exists. This is local source validation, not an
assertion that the pending feature commit has already been published.

## Concurrent collection v0.2.11 cutover — 2026-10-20

A fresh clone starts at published `origin/main` commit
`e0ff164696d86eaca42e218623f6c9f03798f0ad` (`v0.2.11`). That concurrent release
changes only collection release metadata, the three inherited source-reference
bullets and its cleanup-baseline evidence. Preserve those published bullets and
records exactly. The prior isolated feature candidate for `0.2.11` was never
published; its plan, checks and review remain historical evidence.

The accepted entrypoint and evaluation files were copied byte-for-byte, and usage
instructions before the source-reference section were retained byte-for-byte.
The final source-reference section now matches published upstream exactly. The
only difference from the previously reviewed skill is the publication-reference
bullet's label/links, with no change to commands, rules or evaluation cases.
Prepared this feature's pending `0.2.12` VERSION/changelog entry without moving or
rewriting any published release entry. No new behavioral evaluation is needed for
this source-link and release-metadata reconciliation; prior evaluation scope and
live-host limitations still apply.

**Final application commit and published release: pending.** No collection commit,
tag, push or skill installation was performed in this integration. The earlier
isolated clone and original dirty source checkout were preserved.

Executed checks for this cutover:

- Refetched `origin`; local `HEAD` and `origin/main` both resolve to
  `e0ff164696d86eaca42e218623f6c9f03798f0ad`. No local or remote `v0.2.12` tag
  existed at inspection time.
- Reviewed the full diff against that remote baseline: only the three accepted
  skill changes, pending release metadata and the three task records differ.
  Every other tracked upstream file, including cleanup-baseline records, matches
  its published bytes. Published changelog entries remain unchanged.
- Byte comparisons confirmed unchanged accepted entrypoint/evaluation content,
  unchanged usage instructions, and exact preservation of the upstream usage
  source-reference section.
- Discovery listed all four collection skills without installation. Strict
  portable frontmatter/unique-key checks passed for all four; Skill Creator
  validation passed for the changed package.
- Collection Markdown lint checked 47 files with no issues. Repository
  Markdown/local links checked 46 files, and package-root links checked three,
  with no issues. All 10 unique current application source paths and anchors
  map to existing content in the merged application checkout.
- Both documentation-only architecture validator phases and `git diff --check`
  passed. No runtime, live-host evaluation or publication is inferred from these
  documentation and preservation checks.

## Final application delivery

The application was published as annotated tag `v0.36.0`, commit
`42ce196bb4b91d8d4e80fa59397b730bb0d10cd2`, after the integrated source commit
`dfb3a1606f045deb5326a46da94ef18a8618a00f`. Required contributor and release
hooks passed, including both full routine suites. The final wheel and source
archive match the tagged source; installed-byte, metadata, adapter and dependency
checks passed in four default/minimum environments across Python 3.12 and 3.13.
Remote master and the peeled annotated tag were verified at that release commit.

The [released input contract](https://github.com/BuckanovNikita/clearml-yolo/blob/42ce196bb4b91d8d4e80fa59397b730bb0d10cd2/specs/001-clearml-ground-truth/contracts/input.md)
now governs these skill instructions. The earlier pending application notes above
are historical; the application publication prerequisite for collection `0.2.12`
is satisfied. The latest reconciliation changes only source links and release
evidence, preserving the independently reviewed and evaluated skill behavior.
No live OpenCode/DeepSeek execution or skill installation is claimed.
