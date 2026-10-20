# Baseline reference maintenance: 2026-10-20

The application is consolidating its pre-reset Spec Kit records into
`specs/000-baseline/spec.md`. This task redirects the native configuration, stable parameter/cache and publication links in the portable usage reference. Existing examples,
frontmatter and evaluation cases are unchanged. The adjacent
[architecture record](architecture.json) records inspected documentation-only impact.

This checkout starts from `351fde30139095668d40c7ee917b6276704bdc5e` (v0.2.10).
That published commit already contains stable-cache guidance and its prior evaluation
and review evidence. Concurrent edits in another checkout are outside this patch.
The application baseline is inspected locally before publication; this collection
release must follow the application cutover so its external destinations exist.
No skill installation is requested or performed for this companion package.

## Checks and review

- Architecture plan validation: passed before reference changes.
- Package validation, four explicit Markdown documents, discovery (all four skills),
  45-file Markdown lint, architecture acceptance and whitespace checks: passed.
- Integrated review identified an additional stale publication-contract link in v0.2.10;
  redirected it to the current publication/lifecycle baseline section. The final two-file
  documentation recheck passed; the integrated reviewer accepted the corrected links.
- Application cutover `228be7f4e8184667f39cb1ad797c4b00e6958022` is published on master.
  Its baseline preserves history at `d74945c83dc24094c599388bbcfd5e987daeb9bf`;
  all three destination anchors exist in the checked baseline. Collection delivery
  uses the Git-only v0.2.11 workflow after application publication.

Only external link destinations changed; no new behavioral claim or live OpenCode
model-routing result follows from this maintenance.
