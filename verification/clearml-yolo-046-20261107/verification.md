# CY 0.46.0 guidance reconciliation — 2026-11-07

Source-only collection maintenance. Installed exports, host settings and routing
are outside this task. The [architecture record](architecture.json) records an
inspected documentation-only impact.

## Scope

- Carry the selected DEIMv2 L/X, forced-estimation and contextual-diagnostic
  guidance and its [historical evidence](../dinov3-selected-20261105/verification.md)
  from the uncommitted 2026-11-05 companion draft onto collection `0a3ed5e` (0.7.4).
- Relabel “pending/candidate CY 0.46” and “released 0.45.1 lacks `--force`” wording
  to the published CY 0.46.0 behavior, without dropping native-gate caveats.
- Merge verified points from the 2026-11-03 force and 2026-11-04 closure drafts:
  `--list` rejects `--force`; explicit `model.train.weights`/`backbone_weights`
  suppress the HF fallback; CPU conversion or strict loading is not optimizer,
  augmentation or DDP acceptance.
- Route the entrypoint force note to CY 0.46.0; bump the collection to 0.7.5.

## Source identity

CY tag `v0.46.0` at `60edd4b0fa7c76b209a38b17490d0457b8dee469`; its
`pyproject.toml` version is `0.46.0` and its changelog entry (2026-11-06) lists
“Add forced estimation and official HF DINOv3 weights”. All claims below were
read from that tag with `git show v0.46.0:<path>`, not from a working tree.

## Claim verification against v0.46.0

- `docs/batch-estimation.md`: `--force` row in the option table; listing rejects
  `--imgsz`, `--devices`, `--jobs`, `--refresh` and `--force`; the forced section
  gives `cy-batch-estimate --imgsz 960 --devices 0 --force`, retained foreign-PID,
  telemetry, identity, memory, finite, optimizer and confirmation checks, no
  stopped processes, forced-only `settings.force` persistence with ordinary records
  omitting false, `forced/background-sensitive` labels on cached and extrapolated
  recommendations, compatible training reuse with provenance and the
  strict-reader upgrade requirement.
- `src/clearml_yolo/core/batch_estimation.py` (`model_catalogue`/`select_models`)
  with `core/models.py` `PRESETS`: the estimator catalogue registers
  `deimv2/s|m|l|x`, so `--models deimv2/l,deimv2/x` is a valid selection.
- `src/clearml_yolo/entrypoints/batch_estimate.py`: listing rejects every explicit
  measurement flag other than `--list`, `--models` and help.
- `docs/model-assets.md`: bulk preparation excludes DEIMv2 L/X; selected L/X
  training or estimation prefers a validated original Meta `.pth`, then the pinned
  `facebook/dinov3-vits16-pretrain-lvd1689m` (L) or
  `facebook/dinov3-vits16plus-pretrain-lvd1689m` (X) snapshot with license
  acceptance and `HF_TOKEN`/token file; offline reuse of a complete snapshot with
  `config.json`; CPU conversion to `hf-dinov3-to-deim-v1.pth` with separate
  identity and source/derived provenance; explicit weights retain precedence; no
  new flag; timm Faster R-CNN/FCOS weights unchanged. Token handling through a
  secret manager or protected token file follows the same document's
  authentication section.
- `src/clearml_yolo/adapters/assets/execution.py`: only training with
  `pretrained` enabled and no explicit `weights`/`backbone_weights` reaches the
  L/X fallback; missing explicit files raise `FileNotFoundError`; prediction
  (`training=False`) requests no backbone; only a deep execution copy receives
  resolved paths.
- `src/clearml_yolo/adapters/torchvision/training.py`: nonfinite total loss raises
  before backward/update with the zero-based `enumerate` batch index, target image
  IDs and loss components. `src/clearml_yolo/adapters/observability/capture.py`:
  nonfinite native scalars raise with title, series, iteration and value.
- `docs/bugs/dinov3-hf-native/fix.md` and `test.md`: six selected L/X native
  training cases passed; selected L/X estimator acceptance and the complete forced
  64-model native gate remain open. The guide keeps both caveats.

## Executed checks

Collection base `0a3ed5e16d06ff478b9e067f57b601a3a9823e2b`; all checks ran on
the task branch working tree before commit.

- Shared `module-architecture-planning` validator: plan phase before the evidence
  prose and acceptance phase after scope reconciliation, both exit 0.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: four expected skills
  found, exit 0; list-only, nothing installed.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md'
  'verification/**/*.md' 'docs/**/*.md'`: 89 files, zero issues, exit 0.
- `git diff --check`: exit 0. The commit ran with hooks enabled; this collection
  configures no `core.hooksPath` and no active `.git/hooks`, so none executed.
- Strict frontmatter check of all four packages: unique keys, name/folder
  agreement, known fields and description length passed.
- CY `scripts/check_docs.py` extracted from `v0.46.0`, run with `python -I` from
  the CY environment: package root with `SKILL.md` and both changed references,
  three files, zero issues; collection root with both evidence files and
  `CHANGELOG.md`, three files, zero issues.

## Limitations

No OpenCode, target-model or fresh independent behavioral evaluation ran in this
bounded task; source checks do not establish host routing or model performance.
No native training, GPU job, download, ClearML stand, installation, tag or push
was performed. Independent review and the collection release remain owner tasks.
