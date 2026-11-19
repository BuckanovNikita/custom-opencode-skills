# corp-skills agent instructions

## Purpose and layout

This repository (GitHub `BuckanovNikita/custom-opencode-skills`) versions OpenCode
model-routing, orchestration and skill-authoring knowledge for DeepSeek, GLM and
Qwen models. It contains no executable code, package manifest or dependencies.

- `AGENTS.md`: always-loaded rules (this file).
- `DOCS.md`: reference knowledge, read on demand: orchestration, authoring contracts,
  model runtime facts, release script.
- `README.md`: Russian user-facing overview.
- `CHANGELOG.md`, `VERSION`: release record and the single collection version.
- `.markdownlint.json`: Markdown lint config (`default: true`, MD013 off).

Do not recreate skill packages, skill directories or instructions that tell the
reader to use a skill.

## Environment

Node.js with `npx` and Git. No install step; `npx --yes` fetches pinned tools into
the npm cache.

## Checks

Run from the repo root before a PR is marked ready:

```bash
npx --yes markdownlint-cli2@0.23.3 '*.md'
git diff --check HEAD   # first `git add -N` task-owned new files
```

After committing, `git diff --check "$(git hash-object -t tree /dev/null)" HEAD`
checks the whole tree. Before the next release, rerun all checks from a fresh clone
of `main`.

If JSON files are added, confirm they parse (for example `node -e 'JSON.parse(require("fs").readFileSync(process.argv[1]))' FILE`).

## Hard constraints

- No local checkout path, private endpoint, credential value or machine capacity in
  committed files.
- Linear is private and this repo is public: in commits, PR text and committed files
  cite an issue only by identifier `SIP-NN`; never paste Linear URLs, issue text or
  comments. Put the PR link into the Linear issue.
- Never claim live target-model behavior from linting, discovery, parsers or
  native-agent (Claude/Codex) evaluations; say "untested" when the OpenCode runtime
  was not exercised.
- Model guidance is engineering inference, not measured ranking. Never silently map
  the requested DeepSeek revision `0117` to the documented `1017` or invent dated IDs.
- Label superseded model guidance as superseded in place; do not rewrite recorded
  results.

## Delivery

- Active issues and plans live in Linear team `SIP`, project `custom-opencode-skills`.
  Search for an existing issue before creating one. An issue awaiting triage sits in
  Triage with `needs-triage`; a Feature is a parent issue with sub-issues; blocking
  uses Linear's native blocked-by relation.
- Lifecycle states: Triage, Backlog, Todo, In Progress, In Review, Done, Canceled,
  Duplicate. Move to In Progress when work starts, In Review when implementation and
  selected checks are ready, Done only after the PR is merged to `main` and
  acceptance criteria are met; work that changes no files completes on its recorded
  acceptance outcome. An agent finding an issue In Review with a merged PR checks its
  acceptance criteria and moves it to Done. Record the checks actually run in the issue.
- Start dependent work only when its prerequisite is implemented and verified on the
  base branch; the dependency still orders the merges. Confirm a GitHub check, branch
  protection or Linear automation exists before relying on it.
- The fresh independent review happens before the PR is marked ready; the human merge
  is a separate step.
- Labels (team `SIP`, use existing ones, never create lowercase duplicates): exactly
  one readiness label (`needs-triage`, `needs-info`, `ready-for-agent`,
  `ready-for-human`, `wontfix` + state Canceled) and one category label (`Bug`,
  `Feature` for new capability, `Improvement` for existing behavior). A ready label
  never overrides an open blocked-by relation.
- One task branch and one PR against `main` per cohesive change; the issue identifier
  goes in the branch name and PR title; list every included `SIP-NN` in the PR
  description. PRs deliver Linear issues; they are not a request surface.
- Agents push task branches to `origin` and open PRs; humans merge. Never push
  `main` and never deliver by merging into `main` locally. The only exception is the
  separately authorized release push below.

## Versioning and release

- One SemVer for the whole repository in `VERSION`. Pending changes go under
  `## [Unreleased]` in `CHANGELOG.md`; a release moves them to
  `## [X.Y.Z] - YYYY-MM-DD` and updates `VERSION`. Bump deliberately, never infer.
- Patch: compatible corrections and wording. Minor: new compatible content. During
  `0.x` a breaking change (removed or renamed content, changed contracts) is a minor
  bump with documented migration; `1.0.0` declares the stable contract.
- Releases are annotated tags `vMAJOR.MINOR.PATCH` only. Tags are immutable: never
  move, delete, force-push or recreate a published tag; fix with a new patch. Never
  create GitHub Releases, upload assets or publish to a registry.
- Release only after committing the release files on `main` and getting separate
  explicit push authorization. Run with Bash from the repo root:

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
test -z "$(git ls-remote --tags origin "refs/tags/$release_tag")"
git tag -a "$release_tag" -m "Release $release_tag"
git push --atomic origin HEAD:refs/heads/main "refs/tags/$release_tag"
```

- If `origin/main` is not an ancestor of `HEAD`, reconcile and rerun checks first.
- If the push fails the local tag may remain: inspect local and remote refs before
  retrying the same push. Afterwards verify the annotated tag resolves to the intended
  commit and remote branch and tag refs equal local refs; report commit, version,
  checks and limitations.

## Writing standards

- Markdown must pass the lint command above; JSON must parse; no trailing whitespace.
- State rules and facts directly: outcome first, required inputs, conditional actions,
  result shape, acceptance evidence. No repeated emphatic rules, no requests for
  hidden reasoning, no raw model control tokens, no R1-era prompting advice.
- Runtime parameters (model, thinking, effort) belong to the host adapter config,
  never to prose.

## Pitfalls

- `deepseek-flash` is a moving API alias, not an OpenCode provider/model ID; take IDs
  from `opencode models`.
- OpenCode Plan is an agent, not an enforced read-only sandbox.
- A worker's success message is not verification; inspect the integrated diff.

See [DOCS.md](DOCS.md) for orchestration, authoring and model runtime reference.

<!-- markdownlint-disable MD022 MD032 -->

## Working agreement
- At the start of work, state the worktree path, the current branch and its relation to the default branch; say when remote refs were not fetched.
- Inspect existing and staged changes before editing. Preserve unrelated work and local overrides; stage only task-owned paths and inspect the staged diff.
- Report what each executed check establishes and what remains unverified; distinguish mocked checks, live outcomes and historical evidence. Clean up task-owned processes and temporary resources.
- Tests must exercise product code, never project tooling.
- Changes to behavior, interfaces, dependencies, security or operational policy need a fresh independent review after integration and verification. Small wording edits may be self-reviewed.
- Split large work into independent delegated tasks with explicit write ownership and acceptance evidence; delegated agents do not delegate further.
- Use Conventional Commits. Commit automatic formatting separately from logic. Local commits within the task scope need no approval.
- README.md is Russian; AGENTS.md and DOCS.md are English. Keep observed counts, timings and deployment status out of both files.
- Keep machine-specific paths, endpoints and settings in local configuration, not in these files.
