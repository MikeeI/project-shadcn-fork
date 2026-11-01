# ISSUE-043 — registry: namespace discovery stops before newly mapped dependencies

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

Review reference: `E42`.
[S] Discovery scans the currently loadable graph once, then adds mappings without rescanning newly reachable items.

## Reach-and-Impact

[S] A universal root can fail on an indirect namespace even when both mappings exist in package configuration.
The early universal Add path supplies a concrete caller; not every ordinary Add path is proven to fail identically.
No CLI replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/registries.ts:26-57,77-104` separates scanning from mapping addition.
- [S] `packages/shadcn/src/commands/add.ts:101-140` can install a universal item before another discovery pass.

## Prior-Art

Registry-add lookup order is distinct in `ISSUE-045`.
Upstream searches remain pending.

## Proposed-Change

Repeat discovery and mapping accumulation to a fixed point in the existing helper.

## Scope-and-Constraints

Keep components configuration precedence.
Package mappings stay in memory; only eligible directory mappings follow existing persistence policy.

## API-and-Compatibility

Transitive mapping discovery must not silently persist package-owned configuration.

## Verification

Planned smoke: provide package mappings for A and B, with unmapped A/root depending on B/child.
Both must resolve through universal Add without package mappings being written into components configuration.

## Publication-Blockers

A versioned transitive Add replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce transitive namespace omission
Action: Install the universal two-namespace graph with mappings available only from package configuration.
Done-When: Discovered mappings, fetch sequence and unchanged persistent configuration are captured.
