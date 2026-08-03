---
name: git-deliver-work
description: Safely verify, stage, commit, and optionally push current Git work. Use when the user explicitly asks to commit, push, commit and push, save the current changes, or publish the current branch without creating a pull request. Do not use for message-only requests, release workflows, history rewriting, force pushes, or pull request creation.
---

# Git Deliver Work

Finish the exact Git operation the user requested while preserving unrelated work and repository conventions.

## Select the mode

- Treat commit as commit-only.
- Treat push as push-only.
- Treat commit and push as both operations in that order.
- Do not push, open a pull request, create a tag, or change branches unless the user explicitly requested that action.
- Route production release requests to a repository release workflow when one exists.

## Inspect before mutating

1. Read applicable repository instructions such as AGENTS.md.
2. Inspect the current branch, upstream, staged changes, unstaged changes, and untracked files.
3. Read the relevant diff before deciding scope or writing a commit message.
4. Preserve unrelated user changes. Never default to git add -A in a mixed worktree.
5. If the intended commit contains unrelated intents, propose separate commits instead of combining them silently.

## Commit workflow

1. Identify the files that belong to the requested change.
2. Run the repository formatter in write mode on every changed formatter-managed file when repository instructions require it.
3. Run the smallest meaningful checks for the change, reusing checks already completed in the current task when they remain valid.
4. Run git diff --check.
5. Stage only explicit in-scope paths.
6. Re-read the staged diff and confirm no unrelated files entered the commit.
7. Write a concise Conventional Commit message. Use Thai for the subject and body by default, while keeping type(scope): and technical terms in English, unless the user or repository requires another convention.
8. Commit non-interactively and report the resulting hash.

Do not claim skipped checks passed. If a required check fails, stop before committing unless the user explicitly accepts the failure.

## Push workflow

1. Confirm the current branch and remote target.
2. Distinguish committed commits from dirty local files; explain that uncommitted changes are not included in a push.
3. Inspect ahead, behind, and divergence state. Fetch only when remote state must be refreshed.
4. Push the current branch to its configured upstream. Add upstream tracking only when it is missing and the target is unambiguous.
5. Never force-push, rewrite history, move tags, or delete remote refs.
6. Verify the local branch tracking state after a successful push.

Do not create or monitor a pull request or CI run unless the user asks.

## Handoff

Report only what actually happened:

- commit hash and message, when committed;
- pushed branch and remote, when pushed;
- checks that ran;
- remaining dirty files or divergence;
- any action intentionally omitted because it was not requested.
