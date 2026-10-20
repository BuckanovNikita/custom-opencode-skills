# Stable ClearML cache guidance: 2026-10-16

This companion documentation implements clearml-yolo feature
[036: stable parameter identity](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/036-stable-clearml-cache).
Application source was inspected on `master` at base commit
`7dfa425b204f0687311b707e15293d3361e0f177` with feature changes in the working tree.
Collection base: `main`, version `0.2.8`, commit
`70d180c2287f61a89279c6f598f7d2a54401950b`.
These are source checks before delivery; they do not establish an installed release.
The parent workflow records final application/collection commits and release refs.

Final review reconciliation additionally qualified full replay snapshots as CLI-owned,
documented embedding callers' input-recording responsibility and historical General
fallback, and recommends JSON text with `ensure_ascii=False` for Unicode controller
overrides. The installed SDK uses JSON for dictionary job overrides, while its
`Task.set_configuration_object(config_dict=...)` path uses HOCON; the guidance
distinguishes these APIs.

## Scope and architecture

Updated [usage](../../../skills/clearml-yolo/references/usage.md) and
[evaluation prompts](../../../skills/clearml-yolo/references/evaluation.md).
The [architecture record](architecture.json) declares inspected documentation-only
impact. Skill name, entrypoint metadata, invocation policy, executable modules,
dependencies and installed copies remain unchanged. No installation was performed.

The new guidance separates scientific `General` parameters from full sanitized `run`
replay settings. It documents explicit controller callback registration/composition,
CUDA count equivalence, semantic differences, proven prepared-dataset identity,
conservative unsupported-document handling and reuse of previous remote outputs.
Source reconciliation covered `parameters.py`, `cache.py`, `session.py`, training's
prepared-dataset provenance, storage's existing cache identity, remote replay and
the corresponding feature contract. The SDK registration implementation was inspected.

## Executed checks

- Shared architecture validator, `--phase plan`: exit 0 before reference edits.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0; discovered all
  four collection skills, including the unchanged clearml-yolo name/description.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`:
  exit 0; final run checked 40 files with zero issues.
- Codex skill-creator `quick_validate.py` against the source package: exit 0,
  `Skill is valid!`.
- YAML frontmatter validation with duplicate-key rejection, supported metadata,
  description/name constraints and folder agreement: passed. All four packaged local
  links resolved within the package; Markdown files were not external symlinks.
- Application read-only Markdown checker against the skill entrypoint and both
  references: three files, zero issues. An initial invocation used relative paths
  from the application directory and correctly rejected paths outside the supplied
  collection root; rerunning with absolute paths passed.
- Extracted the exact registration examples from application `TECH_DETAILS.md` and
  companion `usage.md`, compiled and executed both in a temporary Python process:
  the installed SDK registered the intended callback; CUDA index/output relocation
  projections matched; an epoch change differed; input dictionaries stayed unchanged.
- Application read-only Markdown checker against six affected application docs:
  six files, zero issues. `git diff --check` passed in both repositories.
- Shared architecture validator, `--phase acceptance`: exit 0 after scope reconciliation.

## Remaining acceptance and delivery

Two fresh native-agent contexts evaluated variants of the new success/failure
prompts without reading expected answers. The success response gave controller-side
registration and explicit callback composition, equal CUDA count, preserved replay
and scientific settings, and remote-output reuse. The failure response rejected
recursive key removal, scientific collisions and worker registration, retaining
historical HOCON and unproven input paths. Both met the documented expectations.
Neither executed workloads or edited files. A third fresh context evaluated the
input-root/model-project/Unicode variant from only the skill and usage guidance.
It correctly retained skipped-stage run roots and fallback lookup projects,
recommended concrete model task IDs, and used explicit Unicode-safe JSON text.
It also explained the remote-output/local-directory distinction. This case passed.
A fourth fresh context correctly qualified native YOLO multi-device equivalence
and rejected RF-DETR/export selector aliases unsupported by their consumers. It
read only the skill and usage guidance; this case passed without executing work.
Fresh independent final review accepted the application and companion changes for
local source delivery, with no blocking implementation findings. It approved the
added caveat that filtered publication does not delete General keys inherited from
historical templates. Application live acceptance and release remain separate gates.
No live OpenCode target-model evaluation was performed, and CLI discovery does not
establish automatic host/model routing.

No ClearML tasks, jobs, training or GPU work ran for these documentation checks.
Synthetic selector equality and registration do not establish a live cache hit or
physical GPU equivalence. Application integration/live evidence belongs to feature 036.

The collection's patch release follows application delivery and its own
[release workflow](../../../RELEASE.md). Version/changelog updates, commits, tags and
pushes remain parent-owned. Reinstallation is separate and was not requested.
