# Selected DEIM L/X guidance — 2026-11-05

## Plan and architecture

The [architecture record](architecture.json) records inspected documentation-only
impact. This task follows the collection maintenance workflow and supplements the
CY `dinov3-hf-native` bug ledger. It changes usage and explanation-only evaluation
references, retaining skill entrypoint metadata and the existing bulk catalogue.
The CY fix is pending acceptance and release; this text must not claim otherwise.

- [x] Inspect current CY routing/conversion, skill references and maintenance rules.
- [x] Validate the no-impact planning record before prose changes.
- [x] Update selected DEIM L/X guidance and success/failure evaluations.
- [x] Check source contracts, package metadata/links, Markdown and list-only discovery.
- [ ] Obtain parent-owned fresh review/evaluation and reconcile final CY release.

## Source identity and boundaries

The collection starts from its current clean source checkout; exact commit and
version are recorded with executed checks below. The companion CY starts at
`66279a6` (0.45.1), with an uncommitted isolated DINOv3 bug fix. Source inspection
covers `adapters/assets/execution.py`, `adapters/deim/assets.py`,
`adapters/deim/hf_weights.py`, `core/assets.py` and `docs/model-assets.md`.
It establishes documented routing and provenance fields; it does not establish
native training, model quality, GPU use, publication or offline execution.

No commits, tags, pushes, skill installation or release metadata changes are
part of this bounded task. The parent owns acceptance and subsequent release.

## Executed checks

Collection source: `0a3ed5e16d06ff478b9e067f57b601a3a9823e2b`, version `0.7.4`.
The task branch changes only the two references and this evidence directory;
collection version, changelog and entrypoint frontmatter remain unchanged.

- Shared architecture validator: plan passed before reference edits; acceptance
  passed after scope reconciliation (documentation-only record).
- Codex `skill-creator` quick validator: clearml-yolo package valid, exit 0.
- Strict YAML check: all four packages passed duplicate-key rejection, recognized
  fields, names/folder agreement, field types and length constraints, exit 0.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: four expected skills
  discovered, exit 0. The command used list-only mode and installed no skills.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'`: 88 Markdown files, zero issues, exit 0.
- CY `check_docs.py` with the distributed `skills/clearml-yolo` package as root:
  entrypoint and both changed references, three documents, zero issues, exit 0.
- CY `check_docs.py` with the collection as root: this task evidence, zero issues,
  exit 0. `git diff --check`: exit 0.

Source reconciliation confirms the L/X guard is limited to training with
pretrained initialization and no explicit detector/backbone weights; the caller's
canonical model is copied. Original Meta reuse is local-only, followed by pinned
Facebook HF resolution. The converter consumes source tensors once, validates the
native inventory/shape/dtype, strictly loads, independently hashes source/derived
bytes and verifies cached tensors before reuse. No new user configuration field
or command flag is introduced, so the changed examples are existing selector
names checked against source rather than claimed command execution.

Fresh-context reading/routing evaluation and independent review remain the
parent's acceptance tasks. This bounded agent did not run OpenCode or target-model
evaluations, native training, GPU jobs, downloads or ClearML publication. Source
review and package checks do not establish those outcomes. The pending wording
must be reconciled to the actual CY tag only after its verified release.

## Fresh independent source review and evaluation

Native agent `review_usage_fresh` reviewed the complete companion diff and
actual CY implementation in a fresh context after parent inspection and the
checks above. Verdict: accepted for source-only documentation, no blocking
findings. Release reconciliation remains pending.

The reviewer separately answered both new explanation-only evaluation prompts
using explicit package reading. Initialization evaluation passed source-support
inspection, pending labeling, selected guards and Meta precedence, canonical
selector preservation, CPU conversion/provenance, bulk catalogue boundaries,
and the prohibition on execution. Its answer described selected pretrained
L/X fallback to the pinned official Small/Small+ source, separate derived
identity and source/derived hashes, with no claim of native acceptance.

Recovery evaluation passed terms and authentication, protected token handling,
no random initialization or hash bypass, representation distinction, repair
prerequisites and tensor integrity, explicit-input errors, and explanation-only
restrictions. Its answer required authorized source access, distinguished
derived serialization from original Meta bytes, and explained atomic rebuild
only with a validated pinned source. No cache or service action occurred.

These are fresh native agent source-based evaluations. They establish neither
OpenCode routing nor target-model performance. Collection release, installed
skill behavior and CY native acceptance remain distinct parent-owned gates.

## Combined release amendment plan — 2026-11-05

The parent selected one combined prospective CY 0.46 release rather than an
intermediate DINOv3 release. Preserve all completed DINOv3 checks/review above;
they do not establish acceptance of this expanded companion diff. Reuse the
same [no-impact architecture record](architecture.json), existing task directory
and maintenance workflow.

- [x] Inspect current force admission/monitoring, key/lookup policy, CLI help and
  scoped campaign diagnostic sources before documentation changes.
- [x] Validate amended no-impact plan before the additional prose.
- [x] Reconcile force guidance/example and add explicit-override evaluation.
- [x] Add bounded numerical diagnostic guidance/evaluation without recovery claims.
- [x] Repeat package, discovery, Markdown/link and source/help checks.
- [ ] Obtain parent-owned fresh combined review/evaluation and release reconciliation.

The current source and frozen prospective release snapshot are parent-owned.
The paired environment remains frozen: this task invokes its interpreter/CLI
for help and documentation checks only, with no package installations or native
workloads. The complete forced 64-model native gate and original 20-case campaign,
including NaNs/SIGILL, remain pending separate application evidence.

### Combined-source inspection and executed checks

The paired environment reports installed CY `0.46.0`; its actual
`cy-batch-estimate --help` exits 0 and exposes `--force` with the description
“allow unattributed GPU activity; retain telemetry and foreign-process checks”.
This is prospective candidate identity, not a published release claim. No
package was installed or updated. The example's options and model selectors
match current help/source; the measurement command itself was not executed.

Inspected source matches the parent's frozen final-candidate snapshot byte for
byte for all five force modules (`core/batch_estimation.py`,
`entrypoints/batch_estimate.py`, `entrypoints/options.py`,
`adapters/runtime/gpu_inventory.py`, `adapters/integrations/batch_probe.py`)
and both campaign diagnostic modules. The source establishes:

- Force changes only the unattributed-utilization checks. Numeric memory,
  utilization and PID conversion, device identity and explicit foreign-process
  rejection remain active. Budget peaks retain observed device baseline and
  native non-allocator accounting.
- `estimate_key` omits false `force` to retain ordinary historical keys; true
  force remains in the key. CLI labels recommendations if any source is forced.
  `compatibility_issues` and `select_recommendation` do not exclude a forced
  source, so compatible training lookup may reuse it; the guide states this.
- Scalar errors include title, series, iteration and value before the journal
  write. Torchvision rejects nonfinite total loss before backward/update with
  zero-based batch, target image IDs and each component. These changes add
  context rather than numerical recovery or changed scientific settings.

All companion package checks were repeated after the combined prose changes:
strict four-package YAML validation, clearml-yolo quick validation, list-only
four-skill discovery, full Markdown lint (88 files, zero issues), the three
package Markdown/local-link checks (zero issues), evidence link validation,
architecture acceptance structure and `git diff --check` all exited 0.
Metadata and skill names/descriptions remain unchanged; discovery was list-only,
with no skill installation. Source-based checks do not establish actual host
routing, live target-model performance or native workload success.

The complete task-owned diff remains the usage/evaluation references and this
evidence directory. No version/changelog, executable helper, application source,
installed skill or host configuration changed. The previous DINO-only review
covers its original scope; fresh combined independent review/evaluation and
actual CY release reconciliation remain parent-owned outstanding gates.

## Release reconciliation — 2026-11-07

CY published this combined work as `v0.46.0` (tag commit
`60edd4b0fa7c76b209a38b17490d0457b8dee469`; its changelog lists
“Add forced estimation and official HF DINOv3 weights”). The pending and
candidate wording in the references was relabelled to CY 0.46.0 by
[the 0.46.0 reconciliation](../clearml-yolo-046-20261107/verification.md),
which records its own checks. The sections above remain historical evidence.
CY's own v0.46.0 records keep selected L/X estimator acceptance and the complete
forced 64-model native gate open; the release does not close them.
