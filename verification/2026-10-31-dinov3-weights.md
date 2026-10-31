# DINOv3 weight-release guidance, 2026-10-31

This documentation-only change accompanies the CY DINOv3 publication amendment in
[feature 009](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/009-release-weights/spec.md).
The isolated collection starts at main commit
`914f81b0d27fe6b2888adb2a4462bec47f90a145`, version `0.6.7`. The parent owns the
candidate CY implementation from master `b23952c433f2addbbdc981fbed1fd8073602cd01`,
the new `weights-2026-10-31` snapshot and its publication evidence. Their released
identities must be reconciled before this guidance is delivered.

## Scope, architecture and tasks

The [architecture record](2026-10-31-dinov3-weights-architecture.json) classifies
these portable prose and evaluation changes as no runtime-module impact. The
inspected CY discovery boundary removes only the `dinov3_small` bulk exclusion;
existing shared-identity deduplication and native Hub restoration remain in CY.
No helper, dependency, discovery metadata, installed copy or original checkout
changes. The parent owns runtime architecture and native acceptance.

- [x] Inspect the entrypoint, relevant usage/evaluation references, collection
  maintenance and release rules, and the parent-provided CY amendment.
- [x] Correct the catalogue and preserve installed-version/snapshot checks.
- [x] Describe individual DINOv3 terms, conversion provenance and attribution;
  retain existing AGPL and DEIMv2 non-commercial qualifications.
- [x] Update explanation-only evaluation cases for inclusion and older snapshots.
- [x] Validate architecture, metadata, packaged links, Markdown and discovery.
- [x] Obtain parent-owned fresh review/evaluation and reconcile final CY identity.
- [x] Follow the collection's Git-tag-only release workflow after acceptance.

## Checks and limitations

The shared architecture validator accepted the no-impact record in plan and
acceptance phases. Codex skill-creator `quick_validate.py` accepted clearml-yolo.
Additional strict YAML validation rejected duplicate keys and checked recognized
metadata fields, field types, names, name/folder agreement and lengths for all
four skills. The entrypoint's metadata and invocation policy remain unchanged.

`DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` discovered all four
collection skills without installation. The configured markdownlint command
passed across 73 Markdown files with zero issues. CY's Markdown/local-link checker
accepted the entrypoint and both changed references with the distributed skill
folder as root, then checked this evidence and CHANGELOG.md against the collection
root; five documents, zero issues. `git diff --check` passed. Collection VERSION
and the dated changelog agree on prepared patch `0.6.8`.

The current CY `adapters.assets.preparation._discover` implementation was inspected:
only DEIMv2 large/xlarge remain excluded. Existing deduplication keeps the two
supported DINOv3 model selections on one native asset identity. This is source
inspection; the parent owns executed packaging, native offline and hosted checks.

The initial checker invocation with system Python could not import Pydantic;
the checks above succeeded with CY's prepared repository interpreter. No dependency
was installed or changed for this correction. No OpenCode executable was available;
no live OpenCode/target-model evaluation was performed. The parent-owned fresh
review must exercise the revised offline-preparation expectations and new
explanation-only older-installation/licence prompt before release. These portable
checks do not establish workloads, training, GPU inference or hosted downloads.

## Release handoff

The bounded documentation task created no commits, tags, pushes or installations.
After parent review, evaluation and final CY/snapshot source reconciliation, use
the collection's existing [release procedure](../RELEASE.md): inspect and stage
only these task-owned paths, commit the prepared patch on `main`, verify clean
state and unchanged remote ancestry, create annotated `v0.6.8`, push main and
that tag atomically, and read back remote refs. Publish CY's implementation before
claiming its amended catalogue is released. GitHub Release objects, package assets
and installed-copy updates are outside this collection release.

## Final parent acceptance and delivery

A fresh read-only native reviewer repeated architecture, metadata, discovery,
Markdown and local-link checks and exercised both revised reading evaluations.
It correctly distinguished a hosted weights snapshot from an installed CY upgrade,
retained individual DINO/YOLO/DEIM terms and attribution, required actual hosted
bytes and fresh-cache native loading, and performed none of the explanation-only
prompt's forbidden actions. It returned ship conditional on the now-completed
source/publication identity reconciliation. This is native-agent reading evidence,
not a live OpenCode/target-model evaluation.

CY [v0.44.5](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.44.5) was
published with release commit `d4f566cbd79839be8d6b0767e11cb349ddf4ab8e`,
following feature commit `d2761725835dae6fd5d8605308f9ccbe3029e7dc`. The
annotated tag and master refs were pushed atomically and read back. The actual
release source/installed metadata match queue 0.1.6 at
`2fbaed25fa7c572757d30cb9b1b343d22e645e3a`; normal resolution, pip consistency
and 544 queue tests passed, with seven physical opt-in tests deselected.
Both complete CY canonical runs passed 4,571 tests; current-context source and
normal commit/version hooks passed without bypass.

The public [weights-2026-10-31 snapshot](https://github.com/BuckanovNikita/clearml-yolo/releases/tag/weights-2026-10-31)
contains 57 payloads (56 weights plus the DEIM source). Actual public restoration
verified all archive/member hashes and genuine native Hub metadata through four
archive downloads, with no upstream model-host requests and zero failures. Native
DINOv3 loaded and produced finite CPU outputs offline. A fresh process verified
the actual packaged pin without a manifest override. Complete DINO agreements,
model card, attribution and timm-conversion provenance remain in the archives.
The [dated CY evidence](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/009-release-weights/dinov3/verification.md)
records scope and limitations; no GPU training, model-quality or live Artifactory
claim is made.

Final parent checks below passed after this evidence update; release 0.6.8 uses
only the six reviewed documentation/version paths and an annotated Git tag. No
GitHub Release, package, release asset or installed-skill update is created here.

The parent's first final link-check CLI invocation defaulted to the CY checkout
root and correctly rejected an external collection document. Repeating it with
`--root .` selected this collection explicitly; no checker or source change was
needed. Final Markdown and local-link checks passed with that correct root.
