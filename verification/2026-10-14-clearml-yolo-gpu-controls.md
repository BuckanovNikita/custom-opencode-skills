# Pending clearml-yolo GPU controls: 2026-10-14

Source collection: `main`, version `0.2.0`, base commit
`1e0ed14ff1346eb168208118e089635e0f575351`.
Tool source: `clearml-yolo` master, package `0.28.0`, base `665dfd3`,
uncommitted feature `031-test-quality-gpu-controls`. The corresponding native
workflow owns implementation and remote physical acceptance; new examples remain
explicitly unreleased. No installation or publication was performed.

Changed package paths: `SKILL.md`, `references/usage.md`,
`references/evaluation.md`. They document physical operator indexes versus
CUDA-logical submission, owner holds, reason/token and whole-allocation release,
force refusal/no signals, admission fencing and early supervision failure.

## Executed checks

- `DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list`: exit 0;
  all four collection skills discovered, including clearml-yolo. List mode did
  not install a skill.
- `npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md' 'docs/**/*.md'`:
  exit 0 on the final 29-file recheck, including this evidence.
- Safe YAML parsing rejected duplicate keys; verified name/folder equality,
  description length, allowed metadata and all three packaged local links.
- Actual `cy-queue gpu --help` and `gpu release --help` match documented arguments.
- Native bounded read-only agent applied all three new prompts after explicitly
  reading the skill; no source discrepancy found. Detailed application evidence
  is recorded with feature 031. This checks explicit skill use, not automatic
  routing or live OpenCode/DeepSeek behavior. Prompts were applied separately
  within one fresh native agent context, not three cold contexts.

## Acceptance boundary

The tool's physical multi-GPU/NCCL/ClearML scenarios await user-returned results.
No GPU or queue control operation was performed for this documentation check.
Collection publication follows tool publication; no new interface is claimed as
released. The [architecture record](2026-10-14-clearml-yolo-gpu-controls-architecture.json)
captures the inspected documentation-only effect.
