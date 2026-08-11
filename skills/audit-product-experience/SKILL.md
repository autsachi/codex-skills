---
name: audit-product-experience
description: Audit a product experience before changing UI code. Use for UX reviews, responsive behavior, mobile/tablet/desktop or device-specific experiences, confusing flows, duplicated views or modes, reader/editor layouts, control density, gestures, navigation, and interface simplification. Establish user goals, usage contexts, capability boundaries, and an interaction contract first; require an approved mockup before structural UI changes; implement only when explicitly authorized.
---

# Audit Product Experience

Start with what the user is trying to accomplish and the context in which they do it. Treat responsive layout as one implementation tool, not the product model.

## Core rules

- Optimize for user understanding and task completion before visual polish.
- Distinguish product experiences, internal states, entry contexts, and layout variations.
- Do not assume every device or usage context needs feature parity.
- Do not remove or add capabilities solely because a breakpoint makes them fit.
- Prefer one coherent interaction model over duplicated pages or modes.
- Give each gesture one unambiguous meaning within an interaction context.
- Preserve established behavior unless evidence or an approved contract requires change.
- Respect the active collaboration mode and the user's authorization boundary.
- Do not implement structural UI changes before the user approves a reviewable mockup.

## 1. Ground in the real product

Before proposing changes:

1. Read repository instructions and the relevant product documentation.
2. Identify the framework, routes, shared UI primitives, styling system, tests, and browser-testing surface.
3. Trace how the target audience enters, completes, and exits the flow.
4. Inspect the current interface, state ownership, URLs, and related contexts.
5. Resolve discoverable facts from code and evidence before asking the user.

When the request is review-only, keep the investigation non-mutating. When implementation is requested, still complete the experience contract before editing UI code.

## 2. Define the product model

Describe the minimum concepts the user must understand:

- the object they are working with;
- their primary goal;
- meaningful usage contexts;
- genuinely different experiences;
- temporary or internal states;
- contextual navigation;
- layout-only variations.

Assume two flows belong to one experience when they share the same goal, content, controls, and state. Keep them separate only when the operating mechanics or required capabilities materially differ.

Do not infer a device experience from viewport width alone when it must remain stable across orientation or window changes. Choose a stable observable signal, document its fallback, and preserve user state when classification changes.

## 3. Build a capability matrix

For each meaningful audience or usage context, record whether a capability is:

- primary;
- available but secondary;
- intentionally unavailable;
- shared across all contexts.

Use the matrix to decide capability boundaries before arranging controls. Explain why a difference exists in terms of user goal, environment, safety, available space, input method, or continuity—not merely screen width.

Within each experience, use responsive behavior for density, placement, stacking, overflow, and progressive disclosure without inventing another product concept.

## 4. Audit interaction architecture

Inspect:

- primary and secondary actions;
- navigation, browser/system Back, and deep links;
- manual and automatic scrolling;
- tap, swipe, drag, pointer, and keyboard behavior;
- focus, dialogs, overlays, and scroll lock;
- loading, empty, error, pending, and offline states;
- state restoration across navigation, reload, orientation, and backgrounding;
- fullscreen, wake lock, media, or immersive behavior when relevant.

Reject gestures with competing meanings in the same context. Keep accessible explicit controls for important gesture actions. Define thresholds, ignored targets, boundary behavior, and state transitions when a gesture is part of the contract.

## 5. Inspect representative conditions

Use repository-defined targets when available. Otherwise inspect only the relevant subset of:

- 360px small phone;
- 390–430px common phone;
- 768px portrait tablet;
- 1024px landscape tablet or small desktop;
- 1280px desktop;
- 1440px and wider desktop.

Also inspect relevant height, orientation, safe-area, keyboard, reduced-motion, touch, pointer, and long-localization conditions.

Use the real browser-testing surface when available. Do not claim visual success from static code inspection or DOM tests alone. If visual inspection is unavailable, state that limitation.

## 6. Prioritize evidence

Classify confirmed findings:

- **P0:** blocks task completion or causes destructive/incorrect behavior;
- **P1:** major usability, comprehension, accessibility, or continuity problem;
- **P2:** meaningful friction, density, discoverability, or responsive issue;
- **P3:** polish with limited effect on task completion.

Separate product-model, interaction, adaptive-layout, accessibility, and visual findings. Do not let P3 polish drive architecture changes.

## 7. Propose one experience contract

Recommend one coherent direction and define:

1. concepts that remain;
2. concepts removed or merged;
3. internal states that stop appearing as modes;
4. contexts that need distinct capabilities;
5. the capability matrix;
6. interaction and gesture rules;
7. responsive behavior within each experience;
8. URLs, data, state, and accessibility behavior to preserve;
9. explicit out-of-scope items;
10. acceptance and regression criteria.

Present the current mental model, confirmed problems, proposed model, contract, preserved behavior, and implementation order. Ask only for product decisions that cannot be derived from evidence.

Once approved, treat the contract as stable. Reopen it only for a new requirement, confirmed usability or accessibility blocker, technical constraint, or explicit user request.

## 8. Require a mockup gate

Create a reviewable mockup before changing product UI when the proposal changes any of these:

- navigation or information architecture;
- named modes or capability availability;
- primary control hierarchy;
- gesture meaning;
- major reader, editor, dashboard, or multi-pane layout;
- a flow spanning multiple screens or states.

Use the available visualization or prototyping surface that best represents the change. Show representative content, affected contexts, responsive/device variants, and important interaction states. Do not present production code as a substitute for approval.

Skip the mockup only for isolated defects, accessibility corrections with an established pattern, copy changes, or an already approved design. Record why it was unnecessary.

Pause structural UI implementation until the user approves the mockup, even if the original request also asked for implementation. Non-visual contract or backend work may continue only when it is independently authorized and does not prejudge the UI decision.

## 9. Implement only when authorized

After explicit authorization and any required mockup approval:

- prefer shared shells, controls, navigation, and state over page duplication;
- implement capability decisions in one discoverable boundary;
- preserve URL, API, server-state, localization, and accessibility contracts unless the approved change says otherwise;
- keep device or context classification deterministic and tested;
- fix structural issues before polish;
- avoid unrelated redesigns, dependencies, or architecture changes.

## 10. Verify and report

Format changed files, run the closest behavioral tests and lint/type checks, and build when shared wiring, routing, build-time classes, or broad TypeScript boundaries change. Run `git diff --check` and inspect the actual interface at the approved representative conditions.

Report:

- final experience and capability decisions;
- meaningful implementation changes;
- viewports, devices, contexts, and states actually inspected;
- commands and checks actually run;
- checks intentionally skipped and why;
- remaining risks or unverified device behavior.

## Anti-patterns

Avoid:

- starting with breakpoints before understanding the task;
- equating responsive design with feature parity;
- shrinking desktop UI until it fits a phone;
- creating modes for temporary state or entry context;
- duplicating a reader or editor solely for another route or parent flow;
- using one gesture for navigation and pagination in the same context;
- hiding essential navigation without an accessible alternative;
- polishing isolated screens before fixing the shared product model;
- implementing a structural redesign before mockup approval;
- declaring success without real interface evidence.
