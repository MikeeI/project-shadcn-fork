# ISSUE-042 — resolver: Theme-first resort reverses a real dependency edge

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

Review reference: `E41`.
[S] An unconditional Theme-first sort runs after dependency sorting and can invert a required edge.

## Reach-and-Impact

[S] A Theme's dependency can overwrite its consuming Theme's own intended values.
The sorter smoke changed correct `B,T` into `T,B`; no public Resolve or installation was executed.

## Evidence

- [S] `packages/registry/src/registry/resolver.ts:310-332` topologically sorts then moves all Themes ahead.
- [S] `packages/registry/src/registry/schema.ts:177-209` permits Theme dependencies and variables.

## Prior-Art

Sibling FIFO priority has an independent sort-stage cause in `ISSUE-041`.
Upstream prior-art search remains pending.

## Proposed-Change

Apply Theme preference only among dependency-compatible candidates rather than resorting across real edges.

## Scope-and-Constraints

Preserve intended baseline priority for independent Themes.
Removing all Theme preference would be an unjustified broader behavior change.

## API-and-Compatibility

Dependency-before-consumer remains authoritative when a Theme explicitly depends on another item.

## Verification

Planned smoke: dependent Theme T overrides B, while an independent Theme retains intended baseline behavior.
Both ordering contracts must hold.

## Publication-Blockers

A versioned public Resolve replay and upstream thread search remain pending.

## Next-Action

Summary: Reproduce Theme edge reversal
Action: Resolve a two-item graph whose Theme depends on a non-Theme item with a conflicting variable.
Done-When: Topological order, post-sort order and final variable value are recorded.
