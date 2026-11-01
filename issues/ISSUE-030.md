# ISSUE-030 — fonts: stylesheet support cache survives later font installation

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: Medium
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E27`.
[S] Font-marker support is cached indefinitely by stylesheet path, including a negative result before a later write.

## Reach-and-Impact

[S] Repeated transformations in one process can remove heading markers after the stylesheet gains heading support.
Current-tree marker union protects an operation that itself declares the heading font.
[A] A versioned supported multi-operation entrypoint still needs a complete replay; separate CLI processes do not prove it.

## Evidence

- [S] `packages/registry/src/transformers/transform-font.ts:24-61,116-123` combines a persistent cache with current markers.
- [S] `packages/registry/src/add-components.ts:92-96,118-140` performs marker-aware installation before later CSS writes.

## Prior-Art

No duplicate cause exists in the local ledger.
Upstream thread and release searches remain pending.

## Proposed-Change

Bound CSS-support caching to one installation or invalidate it after relevant stylesheet changes.

## Scope-and-Constraints

Retain current-tree marker union and avoid claiming an ordinary one-shot CLI failure.
Do not remove font-marker adaptation altogether.

## API-and-Compatibility

A repeated operation must not rely on a stale negative stylesheet result.

## Verification

Planned smoke: cache missing support, install heading font, then transform a component without its own font payload.
All three operations must share a process and the last must preserve heading support.

## Publication-Blockers

A supported-entrypoint multi-operation replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce stale heading support
Action: Establish and replay the three-operation sequence through a supported in-process installation boundary.
Done-When: Entry point, process identity, CSS transitions and final marker output are captured.
