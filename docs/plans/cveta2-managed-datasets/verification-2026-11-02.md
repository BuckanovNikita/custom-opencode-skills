# Managed cveta2 skill verification — 2026-11-02

## Source reconciliation

Initial inspected local source: 3609778906d3e63d0d160cb7d81510f4bf4849db. The source checkout had pre-existing local commits and unrelated clearml-yolo edits; current remote baseline is 2a1869f. A clean remote checkout received only the cveta2 package patch. Three-way conflicts were resolved preserving current upstream diagnostics, timing, incomplete-issue handling and baseline references. The three reconciled package files were copied back only to the task-owned source paths. Other local work remains untouched.

## Checks and review

Frontmatter/name/description/local links and portability validation passed. Discovery lists all four expected skills. Pinned Markdown lint passed on 74 files before this evidence/plan was added; final checks include these documents. The managed YAML example validates against current ProjectConfig; all eight managed API wrappers accept project_config; actual CLI help and documented invocation paths were checked. Parent tool verification includes 2775 local tests, owned mixed-source cold-start live publication/reuse and mutation/architecture gates; final tool release identity is recorded in its Spec Kit evidence.

Independent native evaluator /root/usage_skill_evaluation applied four guidance cases: personal override refusal, arbitrary user script/unmapped errors, uncertain feedback retry and development near-miss routing. All four pass as guidance application in one fresh native context; no claim of four isolated invocations, runtime model effectiveness, OpenCode or DeepSeek execution. The package explicitly requires installed-help/tag checks. An issue-only CVAT timestamp change may cause one conservative annotation refresh on the next fetch.

Final reconciled-package independent review and commit/tag identity are recorded in the release receipt. No installed skill copies were modified.
