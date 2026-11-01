# ISSUE-041 — resolver: descendants reverse declared sibling override priority

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E40`, limited to independent non-Theme siblings without conflicting dependency edges.
[S] FIFO topological sorting lets descendants change the documented last-wins priority of declared siblings.

## Reach-and-Impact

[S] Earlier sibling values can override a later declared sibling.
The review's sorter smoke produced `C,B,A,R` for `R→[A,B], A→[C]`; no public Resolve replay was executed.

## Evidence

- [S] `packages/registry/src/registry/resolver.ts:679-775` determines merge order.
- [S] Declared resolution priority: `apps/v4/content/docs/registry/namespace.mdx:613-638`.

## Prior-Art

The later Theme resort independently violates graph edges in `ISSUE-042`.
Upstream searches remain pending.

## Proposed-Change

Preserve declared priority among independent non-Theme siblings while keeping true dependencies before consumers.

## Scope-and-Constraints

Retain identity, cycle and visited-node handling.
Do not impose sibling order against a real dependency or erase intentional Theme policy.

## API-and-Compatibility

The public resolved tree must retain its documented deterministic override semantics.

## Verification

Planned smoke: A sets brand purple, B sets blue, and R/C set nothing in the four-item graph.
The resolved brand must be blue.

## Publication-Blockers

A versioned public Resolve replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce sibling override reversal
Action: Resolve the four-item non-Theme graph through the public API.
Done-When: Input dependency order, returned merge order and winning brand value are recorded.
