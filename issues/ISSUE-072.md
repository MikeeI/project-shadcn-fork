# ISSUE-072 — mcp: example output omits declared dependency categories

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

Review reference: `E71`.
[S] The example formatter discards dependency fields although the tool explicitly promises examples with dependencies.

## Reach-and-Impact

[S] Clients receive code without declared installation requirements.
The review observed the formatter omission, not a resulting consumer installation failure.

## Evidence

- [S] `packages/shadcn/src/mcp/index.ts:336-339,379-385` promises dependencies and loads complete items.
- [S] `packages/shadcn/src/mcp/utils.ts:144-172` emits only names, descriptions and code.

## Prior-Art

File-content omission is a separate detail-tool contract in `ISSUE-059`.
Upstream searches remain pending.

## Proposed-Change

Emit supplied runtime, Dev and Registry dependency categories next to each example's code.

## Scope-and-Constraints

Preserve optional missing categories and do not invent transitive dependencies.
No guarantee of successful consumer installation follows from listing requirements.

## API-and-Compatibility

Fulfill the existing tool description without introducing a new resolver or installation workflow.

## Verification

Planned smoke: format an item containing all three categories and compare the answer to its payload.
Also check an item without optional dependency metadata.

## Publication-Blockers

A versioned MCP response replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing example dependencies
Action: Request an example whose payload declares all three dependency categories.
Done-When: Original metadata and exact response establish whether every declared category is visible.
