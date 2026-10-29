# Pinned CY weight-release guidance — 2026-10-29

## Scope and baseline

This collection 0.6.3 candidate starts at published collection 0.6.2 commit
`6d8f8be4eb83bcd5ac05eae7386451c6b2dd3667`, fetched from `origin/main`.
Only the entrypoint, model-asset usage section, three evaluation prompts, release
metadata and this dated evidence/architecture record change. Released native model
preparation and local-documentation guidance remain intact. Original shared-checkout
working edits and installed skill copies are outside this isolated candidate.

Companion tool: proposed CY 0.44.0, feature
[009-release-weights](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/009-release-weights).
The fetched CY `origin/master` baseline inspected during preparation is
`0b5f3f725fd65c760f21ea428faf2bbb5e2b6376`, with source version 0.43.0.
The companion 009 changes and expected 0.44.0 are additional pending work, not
features established by that published baseline.
The [manifest contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/009-release-weights/contracts/manifest.md)
and [licence inventory](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/009-release-weights/licenses.md)
are authored in the companion tool candidate. Those links become remotely available
only after its delivery. The new skill prose explicitly labels the CY 0.44.0 mirror
pending until actual publication and installed-version acceptance are verified.

The change explains native-cache reuse before project-mirror restoration and upstream
fallback, independent pinned `weights-*` snapshots, archive/member integrity,
real Hub snapshot/configuration metadata and unchanged source provenance. It retains
catalogue exclusions, explicit selectors, completed partial-attempt retry behavior,
per-file licence obligations and the distinction between model assets and application
distribution/registry publication. Source examples and structural checks do not
establish hosted-byte availability or fresh-cache offline native acceptance.

The [architecture record](2026-10-29-release-weights-architecture.json) classifies the
inspected seven-path scope as documentation-only. Runtime modules, import/dependency
contracts, effects and native acceptance remain owned by the companion CY change.

## Verification and review

Executed checks on the isolated candidate:

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: passed; all four
  expected skills appeared. This was discovery only, without skill installation.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
  'docs/**/*.md'`: passed; 68 Markdown files and zero issues.
- Installed `skill-creator/scripts/quick_validate.py skills/clearml-yolo`: passed.
- Strict YAML/schema validation: passed, preserving the 456-character description
  and name/folder identity. A duplicate-key negative control was rejected. Package
  inspection found no symlinks; local links are covered by the collection check.
- CY `scripts/check_docs.py --root <collection> --all` with the new evidence path:
  passed; 67 governed Markdown files and zero local-link/anchor issues.
- Architecture plan and acceptance validation: passed structurally. The complete
  modified/untracked path set matches all seven paths in the no-impact record.
- Existing evaluation cases remain a byte-identical suffix after the added cases;
  detailed local-documentation and augmentation references are unchanged from 0.6.2.
  Original shared-checkout task-final bytes in all three overlapping skill files
  still match the retained pre-clone digest inventory.
- `git diff --check`: passed. Remote `v0.6.3` did not exist at candidate preparation.

Private command logs are retained under this candidate's Git-private verification
directory. The source CLI and runtime examples were checked against the companion
feature 009 manifest and adapter/packager implementation during authoring; the parent
owns final verification against its integrated CY source and proposed installed pair.

Fresh behavioral evaluation and independent review are parent-owned and pending.
The three new cases are expected
behavior, not recorded model successes. No live OpenCode/DeepSeek/GLM/Qwen results
are claimed. No commit, version tag, push, GitHub Release or skill installation is
performed by this preparation task; the collection remains an unpublished candidate.

## Fixture-only proxy setup amendment

The user's follow-up explicitly limits Artifactory verification to client-protocol
fixtures, with no live Artifactory checks. Usage now gives a clean-process setup for
the Generic GitHub release root, optional original DEIM VCS fallback and optional
HF API endpoint, per-host Requests authentication, HF-only token handling, custom
CA bundles and status/content-error recovery. The added failure prompt retains that
authorization boundary. Existing validated files remain reusable on explicit retry.

Primary protocol references inspected: JFrog's
[remote repository layout](https://docs.jfrog.com/artifactory/docs/remote-repositories),
[VCS commit API](https://docs.jfrog.com/artifactory/reference/downloadvcscommit) and
[Hugging Face endpoint/authentication guidance](https://docs.jfrog.com/artifactory/docs/use-hugging-face-with-jfrog-cli).
The CY adapter source was inspected for URL transformation and standard Requests
authentication/TLS handling. Runtime fixture execution and final integrated-source
checks remain parent-owned; the documentation author performed documentation and
structural checks only. No live Artifactory endpoint, credential, permission or
server-side cache state was tested, and fixture passes must not be reported as such.

After this amendment, CY README/model-asset Markdown and local links passed (two
documents), collection links passed (67 documents), collection Markdown lint passed
(68 documents), and task whitespace checks passed. All Bash fences across the two
CY guides, portable usage and the proposed Russian tutorial passed `bash -n` without
execution. The proposed tutorial remained outside the CY checkout while its parent
canonical check ran; rendering/integration and final tutorial validation remain
parent-owned. Fresh evaluation of the added Artifactory case is pending.

## Independent companion review and Artifactory evaluation

On 2026-10-29 a fresh read-only native reviewer accepted the seven task-owned collection paths with no substantive finding. The reviewer independently validated all 67 governed Markdown files and local links, whitespace and the no-impact architecture record. The third recovery evaluation correctly used GH (not CH), GHR Generic repository roots with the full GitHub release suffix, per-host netrc credentials, optional GH VCS/HF routes, endpoint-scoped HF authentication and CA settings. It distinguished 401/403/404 and corrupt replies, preserved valid native files and checksums, and selected explicit cy-download-weights retry after a partial automatic receipt. This is native-agent behavioral evidence, not a live Artifactory or DeepSeek execution.

The model-assets snapshot was subsequently published at [weights-2026-10-29](https://github.com/BuckanovNikita/clearml-yolo/releases/tag/weights-2026-10-29). GitHub reported all six asset sizes and SHA-256 digests matching the prepared files; the parent is independently checking full hosted-byte restoration before final CY delivery. No collection release assets or installation changes are involved.

## Final companion source and publication evidence

CY created its checked 0.44.0 release commit `8f95e095508ab48e523628f4fd2d106126fa28bc` and annotated `v0.44.0` tag through its native hooks. The complete canonical gate passed with 4,445 tests plus static/import checks and a normal verifier success receipt; final independent code review accepted the unchanged source. The actual public weight download completed with 56 assets, zero failures and exactly four archive transfers, allowing only GitHub/release CDN hosts and blocking all upstream hosts, including download.pytorch.org. All hashes matched and repeat preparation reused the native cache. The collection now describes the implemented CY 0.44.0 interface while preserving an explicit installed-version/source/publication guard; the earlier pending annotations above record preparation history. Collection publication follows the final CY/queue source delivery and does not upgrade installed skill copies.
