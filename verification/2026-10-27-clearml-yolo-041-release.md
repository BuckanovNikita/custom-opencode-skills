# CY 0.41.0 portable usage release, 2026-10-27

The original source checkout starts at `44426a6b6fce058ec9ab37588483eb4bc860ca0a`
(0.3.0). A bounded SSH fetch found published collection 0.4.4 at `af6c7e4`.
The isolated release checkout integrates the task guidance onto that published
baseline, preserving newer dataset selectors, Git workspace warnings, publication
contracts, prior augmentation/portable training guidance and all historical records.
Version 0.5.0 adds compatible CY 0.41.0 usage capabilities; stable skill names,
package layout and invocation boundaries remain unchanged. No installed-skill export,
operational installation or dependency change is included.

## Scope and architecture

The release adds batch estimation/strict saved-batch lookup, FCOS, self-contained
exports, native multi-GPU configuration and augmentation integration to the four
clearml-yolo package files. Existing source-first training, Agent replay, assets,
offline recovery and stable cache guidance are retained from the newer remote.
Collection VERSION/CHANGELOG and this dated evidence are release metadata.

The [architecture record](2026-10-27-clearml-yolo-041-release-architecture.json)
records inspected documentation-only impact. No runtime module, helper, dependency,
composition target or import contract changes. Original dirty checkout content and
untracked plans/verification are preserved; historical records already published
upstream remain unchanged. Plan validation passed before edits.

## Executed checks

- Skills CLI 1.7.1 `add . --list` discovered all four collection skills, including
  clearml-yolo; no installation was requested or performed.
- Markdownlint CLI2 0.23.3 passed the collection Markdown scan with zero issues.
- Skill Creator quick validation passed for clearml-yolo. A separate YAML parser
  rejected duplicate keys, checked supported metadata/name/description and verified
  all 12 packaged local links exist and stay inside the package.
- Source inspection rechecked batch CLI defaults/counts and augmentation profile
  schema routing; `normalize_task_hash` remains the controller callback interface.
  Prior source/help and executable schema checks are retained in the native CY
  [documentation evidence](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/006-batch-training/evidence/documentation.md)
  and [single-GPU follow-up](https://github.com/BuckanovNikita/clearml-yolo/blob/master/specs/006-batch-training/evidence/single-gpu.md).
- Fresh native-agent behavior/schema evaluation previously passed after the
  documented follow-up: workload, mixed hardware and FCOS editable-source schema.
  That evidence distinguishes its initial incomplete answer from the combined
  passing result. This release changes availability labels and retains its safeguards.

## Limits and publication status

Native YOLO fresh confirmation and physical DDP acceptance remain incomplete.
Single-GPU FCOS smoke and schema/composition evidence do not prove all-backend
native acceptance or physical multi-GPU correctness. Guidance marks interfaces as
available from CY 0.41.0 and requires checking the installed interface; it does not
claim those acceptance gaps passed. No live OpenCode/DeepSeek evaluation occurred.

Final isolated-checkout discovery, Markdown (63 files), metadata/link validation,
quick validation, whitespace checks and architecture acceptance validation passed.
The complete diff matches the eight recorded release paths; newer remote guidance
and historical records were retained. Fresh independent companion review returned ship with no blockers after parent
integration and inspection of all eight release paths. The review retained the
native acceptance limitations above.

CY 0.41.0 is published at `e12114535caf399b9e8db9d3dc9693eb64581587`;
annotated tag object `179eee28ad9e758e48651ca7acdf4b31e24fef08` peels to that
commit. The paired queue 0.1.2 source is published at
`ae252ed8e633e7548180577739673ff0506e1c0c`. Parent verified the actual-version
paired check: 512 integration tests and static checks passed. These checks do not
close native YOLO confirmation or physical DDP gaps. CY publication precedes this
collection publication. This record accompanies the authorized Git-only collection
0.5.0 release; commit and tag refs are reported in the release handoff.
