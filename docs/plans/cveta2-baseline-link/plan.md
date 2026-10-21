# cveta2 baseline link correction

Update the dataset-contract reference to cveta2's consolidated specification.
The cveta2 tool owns the baseline; this collection owns its usage reference.
Publish the tool baseline before publishing this correction.

Only the source link changes. Existing commands, skill routing, evaluation cases,
authorization boundaries and local resources remain applicable. New behavioral
or target-model evaluation is not needed for this link-only correction; discovery,
frontmatter, packaged links, Markdown and destination checks cover the change.

Architecture: [inspected no-impact record](architecture.json).
Validation and delivery: [dated evidence](verification-2026-10-21.md).

Release this compatible documentation correction as collection 0.4.2 using the
existing annotated Git-tag-only workflow. Preserve unrelated source changes by
using an isolated clean checkout of the latest origin/main.
