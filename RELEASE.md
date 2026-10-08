# Versioning and releases

The whole collection has one semantic version. `VERSION` records the intended
version of the checked-out content; an annotated `vMAJOR.MINOR.PATCH` tag identifies
a published release. Keep `VERSION`, the changelog entry, and the tag consistent.
Tags are immutable. Do not create GitHub Releases, upload assets, or publish to a
package registry as part of this workflow.

## Choose a version

- Patch: compatible corrections, documentation fixes, or clarified instructions.
- Minor: new skills or compatible capabilities.
- Major: incompatible changes to stable skill names, layouts, invocation contracts,
  or required interfaces. During `0.x`, use a minor bump for breaking changes and
  document migration requirements; `1.0.0` declares the initial stable contract.

Start with `0.1.0`. Record subsequent pending changes under `Unreleased`. For each
release, update `VERSION` and move its notes into a dated changelog section. Use
Conventional Commits; version bumps are deliberate, not automatically inferred.

## Validate and review

From the repository root, with Node.js/npm and Git available:

```bash
DISABLE_TELEMETRY=1 npx --yes skills@1.7.1 add . --list
npx --yes markdownlint-cli2@0.23.3 '*.md' 'skills/**/*.md' 'verification/**/*.md'
git diff --check
```

These commands may cache CLI dependencies but do not install skills. Confirm every
expected skill appears. Validate changed skill frontmatter, unique YAML keys,
name/folder agreement, metadata, local links, and bundled resources following the
[package validation guide](skills/opencode-skill-author/references/validation.md).
Check inactive agent examples separately; they are not skill frontmatter. Keep
provider/model placeholders explicit until an owner chooses to configure them.
Record behavioral checks separately from live target-model execution; do not claim
the latter based on linting or discovery. Obtain independent review for substantive
skill changes and keep dated evidence with the affected skill or under `verification/`.

Before committing, inspect `git status`, the complete diff, and the staged diff.
Stage explicit release paths only, preserving other changes. Commit with a
Conventional Commit message such as `feat: add orchestration skill and versioning`
or `chore(release): prepare v0.1.1`. Do not bypass hooks or clean unrelated changes.

## Tag and push

After committing all intended release files, run this block from the repository
root with Bash. It stops on a dirty tree, malformed version, missing changelog entry,
wrong branch, or a pre-existing local/remote tag. Fetch first to detect remote work;
if `origin/main` is not an ancestor of `HEAD`, reconcile it and rerun validation.

```bash
set -euo pipefail
test -z "$(git status --porcelain)"
test "$(git branch --show-current)" = main
git fetch origin
git merge-base --is-ancestor origin/main HEAD
release_version=$(cat VERSION)
[[ "$release_version" =~ ^(0|[1-9][0-9]*)\.(0|[1-9][0-9]*)\.(0|[1-9][0-9]*)$ ]]
grep -Fq "## [$release_version] - " CHANGELOG.md
release_tag="v$release_version"
if git rev-parse --verify --quiet "refs/tags/$release_tag" >/dev/null; then
  echo "Local tag already exists: $release_tag" >&2
  exit 1
fi
remote_release_tag=$(git ls-remote --tags origin "refs/tags/$release_tag")
test -z "$remote_release_tag"
git tag -a "$release_tag" -m "Release $release_tag"
git push --atomic origin HEAD:refs/heads/main "refs/tags/$release_tag"
```

An atomic push publishes the commit and tag together. If it fails, the local tag
can still exist. Inspect the local and remote refs before retrying the same push;
do not move, delete, force-push, or recreate a published tag. If a published release
needs a correction, prepare a new patch version. If another contributor advanced
the branch, reconcile changes and review release/tag state before retrying.

Verify the annotated tag resolves to the intended commit and that remote branch
and tag refs match local refs. Report the commit, version, checks, and limitations.
