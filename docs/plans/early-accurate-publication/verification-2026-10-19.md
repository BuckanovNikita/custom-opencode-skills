# Accurate configuration and publication guidance: 2026-10-19

This companion describes implemented clearml-yolo feature
[037](https://github.com/BuckanovNikita/clearml-yolo/tree/master/specs/037-early-accurate-publication)
and preserves prerequisite
[036](../stable-clearml-cache/verification-2026-10-16.md) guidance and evidence.
It was recovered in an isolated clone of collection commit
`740906249912671bb17f52edabacca247da27337`, version `0.2.9`, after the earlier temporary
companion checkout was unavailable. Only the existing stable-cache references and its
plan directory were copied from the canonical dirty tree; queue-monitoring guidance
already released in `0.2.9` remains. The canonical checkout was not edited.

Application inspection used the implemented feature 037 snapshot based on
`7dfa425b204f0687311b707e15293d3361e0f177`. This record does not claim that snapshot or
this collection has been released. The parent delivery records own final release refs.

## Scope and source reconciliation

Updated [usage](../../../skills/clearml-yolo/references/usage.md) and
[evaluation cases](../../../skills/clearml-yolo/references/evaluation.md), with the
adjacent [plan](plan.md) and [architecture record](architecture.json). The entrypoint,
name, metadata, invocation policy, scripts, dependencies and installed copies do not
change. No executable module or import contract changes in this documentation collection.
Version/changelog preparation and Git publication remain parent-owned.

The usage reference now distinguishes full active executable `run`, selected-preset
built-in differences in informational `run_overrides`, and early validated training
`General`. It preserves replay, remote overrides, owner-only DDP publication, native
normalization and cache identity boundaries. It explains complete-only artifact timing,
explicit result-contributor completion, skipped comparison, incomplete aggregates,
completed uploads surviving later failure and the final verification barrier.

Source reconciliation inspected Hydra backend/default projection and startup sequencing;
ClearML session sanitization/difference/replay handling and cache projection; result-bundle
declaration/completion, ground-truth conflict and finalization rules; and the maintained
configuration, evaluation and export publication contracts. No new CLI command or option
is introduced in this companion. The existing controller registration example is retained.

## Executed checks

- Architecture planning validator passed before reference edits.
- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list` found all four collection
  skills, including the unchanged clearml-yolo name/description. This was list-only;
  no skill was installed.
- Codex skill-creator `quick_validate.py` reported `Skill is valid!`.
- Explicit YAML validation rejected duplicate keys and checked supported fields,
  names, descriptions and folder agreement. All four packaged local links resolved
  within the package; packaged Markdown files were not external symlinks.
- The exact Python callback registration example was extracted, compiled and executed
  with the implemented application source and prepared SDK. The intended callback was
  registered. Equivalent supported YOLO CUDA selectors and changes only to derived
  `run_overrides` produced equal projections; a weight-decay change differed; inputs
  remained unchanged. No ClearML task, network lookup or workload was created.
- Collection Markdown lint and application read-only Markdown/local-target validation
  passed before the final evidence update; final checks below cover that update too.

## Acceptance and limitations

These checks establish package structure, source alignment and the local example's
behavior. They do not establish automatic host/model routing, live OpenCode target-model
behavior, actual controller cache hits, scheduler admission, physical multi-GPU execution,
training quality or export numerical parity. Those application outcomes require their
own execution evidence; configuration and artifact presence alone are insufficient.

The application feature's dated verification records direct CPU/single-GPU, frozen queue
worker loading, CPU Gloo DDP and Faster R-CNN evidence, and separately records unavailable
scheduled execution, physical multi-GPU and unexercised backend/export checks. This
companion does not convert those limitations into passing results or repeat those runs.

A fresh independent native-agent context evaluated queued-detector precedence and
incomplete-aggregate/later-failure scenarios from only the skill entrypoint and usage
reference, without the expected answers. Both passed: it selected active detector
`model.train`/`model.predict` without unused YOLO defaults, applied selected-preset
comparisons, retained full `run` replay, placed validated remote-overridden General before
preparation, and enforced the contributor barrier and required-upload failure semantics.
It also distinguished optional publication failure and derived-view cache independence.
This is native-agent behavior evidence, not live OpenCode target-model evaluation.
A clarification from that evaluation was checked against `session._override_inputs`:
resolved device bindings remain in `run_overrides` when different from selected defaults;
generated output routing is excluded. Usage now states this and startup publication of
both Configuration Objects. No atomic publication order is promised.

Final independent review remains parent-owned before release.
The final task diff was reconciled as reference prose plus plan/evidence only, with the
stable-cache prerequisite preserved. Both prerequisite and feature no-impact architecture acceptance validators passed.
Final Markdown lint checked 44 files with zero issues; the read-only local-link/Markdown
checker also checked 44 files with zero issues. Package/example checks and
`git diff --check` passed. Application release
must precede collection release under [RELEASE.md](../../../RELEASE.md); installation is
separate and was not requested.

## Final integrated review

Fresh independent read-only agent `/root/release_final_review` accepted the complete
application and nine companion paths with no blocking findings after parent checks.
It confirmed portable guidance matches implemented configuration/replay/cache and
complete-artifact semantics, and independently reran checks for all six changed
companion Markdown files with zero issues. Both companion architecture records
passed acceptance. Application feature 036 now has fresh native controller-cache
proof; paired CPU publication/download evidence also passed. Feature 037 retains
the disclosed scheduled-execution acceptance gap. Collection version 0.2.10 contains
compatible guidance updates and follows the application release. No installation
or package publication is part of this delivery.

## Sequential delivery

The application was released first as
[`v0.35.0`](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.35.0),
commit `accdfb664b2c9f605b026b26d713cae4213e05e2`. Its master branch and annotated
tag were pushed atomically and remote refs were verified before this companion
commit. The exact-tag wheel and source archive passed fresh isolated installation
checks on Python 3.12.13 with unchanged prepared dependencies: all 184 shipped
files matched the tag, all 12 command helps and nine generated command configurations
passed, selected Faster R-CNN/default/overwrite checks passed, and installed runtime
targets resolved without application-source leakage.

Collection 0.2.10 final checks passed after review: 44 Markdown/local-link files,
frontmatter and package containment, the exact controller example and cache identity
assertions, both no-impact architecture records, and whitespace validation. The
application's GPU scheduler gap remains recorded. Delivery uses Git commits and an
annotated tag only; no skill installation or release asset upload is performed.
