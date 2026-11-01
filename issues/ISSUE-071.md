# ISSUE-071 — command block: copy indicator succeeds before clipboard result

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

Review reference: `E70`.
[S] The command copy handler ignores the asynchronous boolean result and immediately marks the operation successful.

## Reach-and-Impact

[S] The UI can show success when both native clipboard writing and fallback copying fail.
No successful real clipboard write is required to reproduce the false indicator.

## Evidence

- [S] `apps/v4/components/code-block-command.tsx:52-67` sets copied state without awaiting the helper.
- [S] `apps/v4/components/copy-button.tsx:34-64,98-113` returns a boolean and already has a correct awaited caller.

## Bug-Reproduction

The 2026-10-30 browser review made writeText reject and execCommand return false on the actual command block.
The success check icon still appeared; instrumentation was restored afterwards.
The browser build and deployed commit were not captured.

## Prior-Art

The global menu copy scope is a separate cause in `ISSUE-068`.
Upstream searches remain pending.

## Proposed-Change

Await the existing boolean result and show copied success only when it is true.

## Scope-and-Constraints

Reuse the existing CopyButton policy; no new clipboard abstraction or error workflow is required.
Preserve timer cleanup and manual text selection.

## API-and-Compatibility

The visible completion state must agree with the helper's existing error contract.

## Verification

Planned browser smoke: reject both clipboard paths, then run a successful control.
The failure case must not show success and the control must still do so.

## Publication-Blockers

A versioned browser/build replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce false copy success
Action: Replay both clipboard failures against the rendered command block with restored instrumentation afterwards.
Done-When: Browser/build, helper outcome and visible indicator are captured for failure and success.
