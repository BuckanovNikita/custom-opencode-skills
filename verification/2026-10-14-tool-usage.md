# Tool usage skills verification — 2026-10-14

## Scope and source evidence

Added portable `cveta2` and `clearml-yolo` packages and a canonical cross-repository
maintenance policy. Updated the collection catalog, release checks and version to
0.2.0. Companion changes extend the tools' existing documentation stages; no
installed skill, vendor workflow template or application runtime is changed.

| Tool | Inspected default-branch source | Version | Companion policy |
| --- | --- | --- | --- |
| cveta2 | `c4b2bb9af305bc8e2c3bd2fa5b2cc11c9404f95c` | 0.9.1 | `.specify/memory/engineering.md` |
| clearml-yolo | `cb1eb5878a8e686d2d1137505ececa4b5bf93558` | 0.28.0 | `AGENTS.md`, `docs/agent-workflow.md`, `docs/current-contracts.md` |

The older cveta2 feature checkout was inspected but not used as the command
authority. The current cveta2 default branch includes both job promotion and
polygon-to-box support. No unreleased clearml-yolo GPU controls were documented.
Existing clearml-yolo local dependency mappings and grid-search notes remain
outside this task. The two tools retain separate native release policies.

## Baseline and prompting

Before authoring, static searches found no tool-specific usage guidance in the
two existing packages. The actual skills CLI listed only `opencode-skill-author`
and `opencode-orchestration`. This establishes a discovery/content gap, not a
measured behavioral failure or model comparison.

The packages use explicit inputs, conditional steps, separated example data,
verified interfaces, concise result contracts and observable completion evidence.
This is engineering guidance, not a proven optimal prompt formula. DeepSeek's
[thinking guide](https://api-docs.deepseek.com/guides/thinking_mode/) and
[tool-call guide](https://api-docs.deepseek.com/guides/tool_calls/) were checked
on this date. Thinking/history/effort configuration remains an adapter concern;
skill prose does not enable it or require visible private reasoning.

## Checks performed

- All three no-impact architecture planning records validated before policy or
  package implementation.
- cveta2 `uv run pytest tests/test_docs.py`: 197 passed. This validates its own
  documentation, not the external skill package.
- Changed cveta2 and clearml-yolo policy/evidence Markdown passed
  `markdownlint-cli2@0.23.3`. Initial clearml-yolo table separator spacing findings
  were corrected in the touched documents; no application change was needed.

- All four packages passed strict YAML metadata/duplicate-key/name/description
  checks and contained local references. A separate inline Python check resolved
  169 collection/changed-policy local links and anchors across 12 documents.
- Actual `uv run --frozen cveta2` help probes passed for root, fetch, upload,
  convert, doctor and s3-sync. They corrected draft assumptions about doctor
  options and s3-sync project selection before acceptance.
- Fifteen clearml-yolo probes passed in isolated `CY_HOME`: root help/composition;
  predict, metrics, val, report, ground-truth, train and comparison composition;
  pipeline, RF-DETR and export configuration generation and composition. Generated
  pipeline checks used explicit CPU/input/output overrides. No configured workload,
  tracking task or queue job was executed. Background CSV behavior was traced to
  `adapters/storage/dataset_records.py`.

The package help/configuration checks performed no live CVAT mutation, training,
GPU execution or ClearML publication. Native release-hook integration checks are
recorded separately from package evaluation.

## Integrated acceptance and independent review

The final skills CLI found all four packages; each new standalone package also
returned exactly one matching skill. Collection Markdown lint passed across 28
files. All three architecture records passed acceptance validation. Parent review
included tracked diffs and all new package, plan and evidence files.

Fresh independent native Codex review (`review_tool_skills`, requested
GPT-6.1 Sol/medium in a fresh context) returned **ship**, with no substantive
findings. Requested model settings are not independent runtime telemetry.
The reviewer checked current implementation contracts and prepared these five
scenarios after explicitly reading the packages:

| Scenario | Observed decision |
| --- | --- |
| cveta2 annotation-only export without remote changes | Prepared noninteractive fail-fast `fetch -p 123 -o export --no-images --no-clearml`; omitted promotion, checked destination and four output partitions |
| COCO conversion with blank splits | Required an existing or supplied split policy before conversion; retained `val` to `valid` mapping |
| Unsupported ClearML offline assumption | Rejected invented `clearml.enabled=false` and `output_uri=false`; distinguished composition from execution |
| Queue preparation only | Prepared separate `cy-queue submit --gpus 1 -- ...` syntax; preserved CPU phases and submitted nothing |
| Required artifact upload fails after local workbooks | Reported incomplete execution and retained diagnostic/output evidence |

These are native-agent preparation/failure evaluations, not executed workloads
or target DeepSeek measurements. A second fresh native agent (`evaluate_routing`,
requested GPT-6.1 Sol/medium) used only YAML names/descriptions for six routing
cases: CVAT export selected cveta2; detector training and queue preparation selected
clearml-yolo; cveta2 development, CVAT administration and direct digital-metrics
use selected neither. It then read the clearml-yolo package for missing-input
`cy-predict`: requested checkpoint/CSV and run identity, invented no inputs and
executed nothing. This is metadata-only selection, not OpenCode automatic loading.
Additional package evaluation scenarios were not all executed.
Temporary clearml-yolo probe output/scratch was removed after evidence capture;
no owned probe processes remained.

## Companion delivery

clearml-yolo documentation commit `665dfd3` passed configured commit hooks and
the post-commit release evaluator. The native changelog hook first regenerated
prior committed documentation history; its diff was inspected and staged before
retry. Python application checks were skipped by their configured file filters,
not reported as passed. No version bump was required. The commit was pushed to
`origin/master`; original local dependency mappings and grid-search notes remain
untouched. No GitHub Release or package asset was created.

cveta2 documentation commit `00a6063` was merged into main as `4e2d82d`, following
its merge-only main policy. Configured commit checks passed, including formatting,
Ruff, import/effect boundaries, mypy, Vulture, coverage pytest, fast mutation, build,
lock and Conventional Commit validation. Both native semantic-release commands
confirmed that 0.9.1 was already released; no new tag was required. The full
pre-push mutation profile passed: 7,996 total, 7,764 killed, 232 allowlisted
equivalents, zero unexplained survivors or other failure categories. Version
drift passed. Its armed live integration gate and remote delivery are reported
below; these are native repository release checks, not target-model or portable
package behavioral evaluation.

The first cveta2 push's live integration gate passed 39 tests (160 dependency
warnings). The gate retained its prescribed `cveta2-main` CVAT project/cloud
storage and MinIO bucket for inspection; test-owned ClearML projects were cleaned.
GitHub closed the idle SSH transport while the gates ran, so the first push failed
after green checks. Remote refs were checked before retrying the push with all
required hooks enabled and command-scoped stronger keepalive.

The retry passed full mutation, version drift and all 39 live integration tests
again, then pushed main successfully. Parent inspection of captured retry output
confirmed the passing pytest/gate summary, native retention/cleanup contract and
remote push result. Remote `main` matches `4e2d82d2c6733ab57248dc4dae411a13117109a5`;
the existing v0.9.1 tag is unchanged. Final integration resources follow the
repository's durable-slot policy, rather than temporary package evaluation setup.

The collection is prepared for annotated Git tag `v0.2.0`. Its release commit and
remote branch/tag identity are verified after this accepted evidence snapshot;
no GitHub Release object, registry publication or skill installation is included.

## Evaluation limits

No configured OpenCode executable was available for live DeepSeek V4.1 Flash
evaluation. Native Codex agents are used for source inspection, scenario evaluation
and independent review; these do not establish DeepSeek behavioral performance.
Each package includes runnable evaluation prompts and expected observations for
subsequent target-host testing. Authoring does not install the skills.
