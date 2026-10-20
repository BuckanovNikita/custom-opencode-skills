# cveta2 reviewed dataset publication guidance

Update only the canonical cveta2 usage package to match feature 008: report-only
verification, explicit cleanup, reviewed preparation and later publication.
Remove retired automatic-fetch-publication instructions. Preserve the operations
skill's exclusions and all unrelated collection work.

Affected files are skills/cveta2/SKILL.md and its usage/evaluation references.
The tool implementation owns policy, interfaces, tests and release. Publish the
tool first; then replace unreleased labels with its observed release version.
Follow MAINTENANCE.md and RELEASE.md for source checks, discovery, Markdown,
independent review, versioning and Git-only publication. Do not install skills.

Architecture impact is none: this is portable prose only, with no executable
helpers, runtime modules, dependencies, composition or import contracts changed.
The adjacent architecture.json records this inspected conclusion.

Acceptance evidence belongs in verification/2026-10-20-cveta2-bundles.md.
The tool's native feature record links the companion commit after delivery.
