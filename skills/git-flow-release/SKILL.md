---
name: git-flow-release
description: Prepare, finish, and publish a repository release that follows Git Flow, with or without an explicit SemVer version. Use when the user asks to create or finish a release branch, infer the next release version, merge a release into the production and development branches, create and push a release tag, or publish the completed Git Flow refs. Do not use for ordinary commits or pushes, pull requests, release-notes-only work, deployments without a Git release, history rewriting, or force-pushes.
---

# Git Flow Release

Prepare and publish one repository release while discovering branch names, version sources, metadata, checks, and remote conventions from the repository instead of assuming project-specific paths.

## Discover repository conventions

1. Read repository instructions and release documentation before deciding the workflow.
2. Resolve the production and development branches in this order:
   - Git Flow config such as `gitflow.branch.master` and `gitflow.branch.develop`;
   - explicit repository documentation;
   - the remote default branch plus an existing development branch.
3. Resolve the release branch prefix and tag prefix from Git Flow config, existing release branches, stable SemVer tags, and repository documentation. Default to `release/` only when no conflicting convention exists.
4. Discover release metadata and verification requirements from repository instructions, CI workflows, release scripts, changelogs, manifests, version files, and roadmap or milestone documents.
5. Stop and ask when production/development branches, tag format, release graph, or version sources remain materially ambiguous. Never impose `main`, `develop`, a `v` prefix, or a specific changelog layout without evidence.

## Resolve the version

- Accept an explicit version or a release branch containing a stable three-part SemVer `X.Y.Z`.
- Preserve the repository's established tag prefix, such as `X.Y.Z` or `vX.Y.Z`.
- Never silently change an explicitly supplied version.

When no version is supplied:

1. Fetch the remote and tags, then identify the highest stable SemVer tag under the repository's tag convention.
2. Inspect changes since that tag and any planned version in manifests, milestones, changelogs, or release branches.
3. Prefer a documented unreleased version when the included work completes that release. Otherwise apply SemVer from the actual change scope.
4. Ask the user when multiple versions remain plausible or the breaking-change boundary is unclear.

Verify that the resolved version is newer than the latest stable release and that its local tag, remote tag, local release branch, and remote release branch do not conflict. Never move or replace a published tag; prepare a new patch or hotfix version instead.

## Gate release metadata

Treat repository-required release metadata as part of the release:

1. Identify every required changelog, manifest, version constant, lockfile, roadmap, generated file, or release-note artifact.
2. Derive entries only from changes included since the latest production tag and follow the repository's rules for user-facing versus internal changes.
3. Include exact proposed release-note text and metadata edits in the confirmation plan when wording requires user review.
4. After confirmation, update metadata on the release branch, format it, run its targeted checks, and commit it using repository conventions.
5. Before tagging, verify that all authoritative version sources and release notes agree with the target version.

When the repository requires no metadata change, state that explicitly in the confirmation plan. Do not invent changelog files or version sources.

## Require confirmation

Always stop for explicit confirmation before switching or creating branches, committing release metadata, merging, tagging, pushing, or deleting a release branch. Fetching and read-only inspection are allowed before this gate. The initial request to “release” or “finish” is not confirmation.

Present one exact release plan containing:

- resolved version, release branch, tag, production branch, and development branch;
- evidence for the version and conventions;
- commits and working-tree changes included;
- release metadata and exact proposed notes, or the explicit no-metadata decision;
- checks already run, targeted checks still required, and the authoritative post-push CI gate;
- refs to push and local or remote release branches to delete.

If any version, content, ref, metadata, or deletion scope changes afterward, present the updated plan and confirm again.

## Run preflight

1. Fetch and prune the release remote, including tags.
2. Inspect the current branch, upstreams, worktree, local refs, and remote refs.
3. Continue with a clean worktree. Include clearly related dirty changes in the confirmation plan without staging them; stop for unrelated, overlapping, or ambiguous changes.
4. Check whether production and development branches are behind or diverged from their upstreams. Do not update them before confirmation and never rewrite history.
5. Confirm the target tag is absent locally and remotely.
6. Locate the release branch locally and remotely without creating, switching, or updating it.
7. Stop for conflicts, ambiguous ancestry, unexpected refs, protected-branch constraints that prevent the workflow, or an existing target tag.

After confirmation, fast-forward production and development branches only when safe. Track an existing remote release branch without discarding commits, or create a short-lived release branch from the up-to-date development branch when none exists. Commit only confirmed metadata or in-scope changes and require a clean worktree before finishing.

## Verify proportionally

- Format every changed formatter-managed file before verification.
- Run repository-required release checks and targeted checks for metadata and application changes not already verified.
- Run `git diff --check` for outgoing ranges.
- Use the repository's CI workflow as the authoritative full gate when documented.
- Do not repeat the complete local CI suite solely for ceremony unless the user requests it, CI is unavailable, or unverified high-risk changes justify it.
- Stop before publishing when required checks fail unless the user explicitly accepts a repository-supported exception.

## Finish the release

Prefer the repository's documented release graph. When none is documented, use this default:

```text
release/X.Y.Z --merge --no-ff--> production --annotated tag--> TAG
                                      \
                                       merge TAG --no-ff--> development
```

Run non-interactive Git operations in this order:

1. Switch to the production branch.
2. Merge the release branch with `--no-ff` and a conventional merge message.
3. Create the annotated release tag on the production merge commit.
4. Switch to the development branch.
5. Merge the tag with `--no-ff` so development contains the exact published release.
6. Delete the local release branch only after both merges and the tag succeed and only when confirmed.
7. Inspect the graph, tag target, branch containment, outgoing ranges, and worktree cleanliness.

If the repository uses a different documented Git Flow graph, follow it and show that graph in the confirmation plan. A Git Flow CLI may be used only when its result matches the confirmed graph; prefer explicit Git commands when the CLI is unavailable or interactive.

## Publish and verify refs

1. Push the production branch, development branch, and release tag to the confirmed remote in one explicit command when supported.
2. Delete the remote release branch only when it existed before finishing, deletion was confirmed, and the release refs were pushed successfully.
3. Fetch or query remote refs and verify every published ref points to the expected commit.
4. Never force-push, rewrite history, move a tag, delete another branch, create a hosting-platform release, or start a deployment unless explicitly requested or required by documented repository automation.
5. Treat successful ref verification as completion. Do not poll CI or deployment by default unless the user asks.

## Report the result

Report only verified outcomes:

- production merge commit and branch;
- annotated tag and target;
- development merge-back commit and branch;
- pushed refs and remote;
- release metadata version and notes status;
- local or remote release branch deletion;
- checks actually run and any asynchronous CI or deployment not monitored.

Never claim a deployment succeeded merely because release refs were pushed.
