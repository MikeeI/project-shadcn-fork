# ISSUE-068 — command menu: global copy handles stale state after dialog close

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: UI
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E67`.
[S] The document copy shortcut reads retained menu selection/payload without checking that the dialog is open.

## Reach-and-Impact

[S] After highlighting a copyable item, ordinary copy outside the closed menu can issue the old copy request.
A request attempt does not prove that it wins a race with native selected-text copying.

## Evidence

- [S] `apps/v4/components/command-menu.tsx:378-422` registers the global handler without an open guard.
- [S] Copy payload persists from the highlight handler at `:142-155`.

## Bug-Reproduction

The 2026-10-30 browser review reported: highlight Accordion, close with Escape, then Ctrl-C without selected text.
Instrumentation captured an attempted write of the previous install command.
The browser build and deployed commit were not recorded, so a versioned replay remains required.

## Prior-Art

Runner and installability defects are separate in `ISSUE-069` and `ISSUE-070`.
Upstream searches remain pending.

## Proposed-Change

Require open dialog and current copyable selection, respect native text selection and reset stale payload on close.

## Scope-and-Constraints

Keep keyboard opening behavior and normal page/input copying.
Only prevent the default action when the menu actually handles copying; preserve listener cleanup.

## API-and-Compatibility

Menu shortcuts must not capture unrelated global copy interactions after their UI closes.

## Verification

Planned browser replay: close and reopen the menu, then copy both menu content and selected page/input text.
Closed-menu copying must not issue a menu clipboard request.

## Publication-Blockers

A versioned browser/build replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce stale menu copy
Action: Repeat the close-then-copy scenario with instrumented clipboard calls in a versioned browser.
Done-When: Dialog state, selection, payload and resulting clipboard-call trace are captured.
