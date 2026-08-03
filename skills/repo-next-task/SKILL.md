---
name: repo-next-task
description: Inspect a software repository and recommend the most valuable next implementation task from current state, recent commits, roadmaps, TODOs, and issue context. Use when the user asks what to do next, asks to inspect the next task, requests backlog prioritization, or says phrases such as "ดูงานถัดไป", "ควรทำอะไรต่อ", or "next task". This is a read-only planning workflow unless the user separately asks to implement.
---

# Repository Next Task

Recommend one concrete next slice using evidence from the repository rather than a generic backlog guess.

## Gather current state

1. Read applicable repository instructions such as AGENTS.md.
2. Inspect the current branch, upstream state, working tree, and staged or unstaged changes.
3. Read recent commit subjects and enough diff or commit detail to understand the work just completed.
4. Locate the repository's planning sources, such as a roadmap, milestones, TODO files, README status, changelog, or issue references.
5. Use a connected issue tracker only when it is available and materially improves the answer. Prefer local sources for a local repository question.

Treat dirty files as user-owned. Identify whether they are active work, documentation drift, or unrelated changes; never edit, stage, or discard them in this workflow.

## Reconcile plans with reality

- Detect roadmap items that are marked pending even though recent commits implemented part of them.
- Detect implementation that is blocked by an unfinished prerequisite, migration, configuration decision, or release gate.
- Distinguish the current milestone from later attractive work.
- Prefer finishing an in-progress milestone before starting the next one unless evidence supports a deliberate switch.
- Separate user-facing work from maintenance, release metadata, and optional someday items.

## Rank candidates

Evaluate candidates by:

1. ability to complete or unblock the current milestone;
2. user value and frequency of the affected workflow;
3. dependency and regression risk;
4. size of the smallest independently verifiable slice;
5. strength of repository evidence;
6. availability of a proportional verification path.

Prefer a small task that can be completed and verified over a broad theme with unclear boundaries.

## Recommend

Lead with one primary task. Include:

- the intended outcome;
- why it is next, citing repository evidence;
- what recent work already covers;
- the exact first slice and the main surfaces it touches;
- risks or user changes that must be preserved;
- the smallest useful verification plan.

Optionally list at most two follow-up tasks in order. State when roadmap status itself needs reconciliation.

Do not edit files, create issues, change branches, or begin implementation. If the user later says to proceed, treat that as a separate implementation request and re-check the working tree first.
