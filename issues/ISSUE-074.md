# ISSUE-074 — registry: generic HTTP errors hide supplied server messages in CLI output

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E73`.
[S] Generic fetch errors receive server text as responseBody, but visible message construction only uses cause.

## Reach-and-Impact

[S] A valid custom 429 message remains in error context yet is absent from CLI diagnostics.
Typed 401/403/404/410 paths differ; no CLI HTTP replay was executed.

## Evidence

- [S] `packages/registry/src/registry/fetcher.ts:77-82,103` passes the server text as the third constructor argument.
- [S] `packages/registry/src/registry/errors.ts:155-189` builds visible text from the fourth argument.
- [S] `apps/v4/content/docs/registry/authentication.mdx:326-354` explicitly promises custom-message visibility.

## Prior-Art

Malformed error-body classification has a separate cause in `ISSUE-067`.
Upstream searches remain pending.

## Proposed-Change

Expose existing server detail for generic errors while preserving responseBody and current cause semantics.

## Scope-and-Constraints

Preserve status codes and specialized error classes.
Do not claim the server text is entirely lost from machine-readable context.

## API-and-Compatibility

The documented CLI diagnostic must include valid server-provided detail without changing error identity.

## Verification

Planned smoke: return a unique 429 JSON message to a CLI item-view request.
Require the exact detail in output and preserve a typed 403 control.

## Publication-Blockers

A versioned CLI/HTTP replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce hidden server detail
Action: Invoke the CLI against a controlled generic HTTP error with a unique valid message.
Done-When: Response body, error context and actual terminal output are captured together.
