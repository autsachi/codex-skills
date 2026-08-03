---
name: responsive-ux-audit
description: Audit and improve responsive web UX across representative mobile, tablet, desktop, orientation, keyboard, and safe-area conditions. Use when the user asks for a responsive, mobile, tablet, cross-device, viewport, overflow, touch-target, or orientation audit, or requests fixes and regression coverage for those issues. Use repository-native UI code and tests; do not redesign unrelated product behavior.
---

# Responsive UX Audit

Find confirmed cross-device blockers, implement the smallest coherent fixes when authorized, and verify them without weakening desktop behavior.

## Establish scope

1. Read repository instructions and identify the frontend framework, styling system, routes, and test tools.
2. Identify the target user flows and shared components before editing page-specific code.
3. Use repository-defined viewport targets when present. Otherwise cover:
   - 360px small mobile;
   - 390-430px common mobile;
   - 768px portrait tablet;
   - 1024px large tablet or landscape;
   - 1280px desktop;
   - 1440px and wider desktop.
4. Include portrait-to-landscape and height-only viewport changes when the flow contains overlays, editors, readers, or persistent state.

## Inspect the real interface

Use the repository's preferred browser-testing surface when available. Exercise authenticated state only through an existing authorized session.

At each relevant viewport, inspect:

- unintended horizontal scrolling or clipped content;
- navigation, sticky controls, and safe-area overlap;
- text wrapping, truncation, and long localized strings;
- touch targets, spacing, and destructive-action focus behavior;
- keyboard focus visibility and reachable controls;
- form actions, dialogs, action sheets, and on-screen keyboard behavior;
- scroll lock restoration after overlays close;
- draft, selection, and navigation state across viewport changes;
- desktop density and capabilities after mobile-first changes.

Use 44px as the default minimum important touch target when the repository does not define another accessibility target.

If browser control is unavailable, continue with code inspection and automated checks, but clearly state that visual verification was not completed. Do not substitute an unrelated browser surface when an applicable browser-control skill forbids it.

## Choose and implement fixes

- Fix confirmed blockers before optional polish.
- Prefer shared primitives when multiple pages have the same issue.
- Keep breakpoints late enough to avoid cramped intermediate layouts.
- Add min-width: 0, wrapping, overflow containment, or responsive stacking only where content behavior requires it.
- Preserve semantic landmarks, accessible names, keyboard behavior, and server-state boundaries.
- Do not include a reader redesign, theme redesign, or unrelated component rewrite unless the user placed it in scope.

When the user asks only for an audit, report findings without modifying files.

## Add regression coverage

- Test behavior and preserved state rather than implementation classes when practical.
- Use structural class assertions only for stable responsive contracts that the DOM test environment cannot lay out.
- Cover the smallest relevant test file.
- Add orientation or viewport event tests when component behavior changes on media-query or viewport transitions.
- Verify safe focus, pending-state locking, and cleanup for overlays when affected.

## Verify

1. Format every changed formatter-managed file in write mode.
2. Run the closest relevant test files.
3. Run lint on touched frontend files when practical.
4. Run a production build when responsive changes add new build-time classes, affect shared exports, or visual browser verification is unavailable.
5. Run git diff --check.

Report the viewports and states actually inspected, automated checks run, confirmed fixes, and any visual or device checks that remain.
