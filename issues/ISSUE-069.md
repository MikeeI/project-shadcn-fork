# ISSUE-069 — command menu: npm preference generates unsupported npm dlx

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

Review reference: `E68`.
[S] Component commands interpolate every selected package manager before `dlx`, including npm.

## Reach-and-Impact

[S] npm users receive a command that npm does not support.
This is a runner-selection defect, not a change to package-manager preference persistence.

## Evidence

- [S] `apps/v4/components/command-menu.tsx:142-149` emits `${packageManager} dlx`.
- [S] Existing mapping uses npx for npm in `apps/v4/lib/highlight-code.ts:52-57`.

## Bug-Reproduction

The 2026-10-30 review observed the menu's npm command and ran `npm --logs-max=0 dlx --help`.
On npm 12.2.1, Linux x64, the invocation returned unknown-command failure with exit code 1.
No package installation was attempted; the browser build was not recorded.

## Prior-Art

Argument quoting is a different output contract in `ISSUE-061`.
Upstream searches remain pending.

## Proposed-Change

Use a pure package-manager runner mapping, including `npx` for npm and the existing supported alternatives.

## Scope-and-Constraints

Do not import the server-side Shiki/crypto module into the client just to reuse its mapping.
Preserve stored preference and default selection.

## API-and-Compatibility

The copied command must use an executable runner for the selected supported package manager.

## Verification

Planned browser smoke: switch all supported managers and check the emitted runner syntax.
Run only harmless help/argv checks until an install is separately authorized.

## Publication-Blockers

A versioned browser/build replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce npm menu command
Action: Select npm in the menu and validate its copied runner with a help-only invocation.
Done-When: Browser build, exact command, npm version and exit result are recorded together.
