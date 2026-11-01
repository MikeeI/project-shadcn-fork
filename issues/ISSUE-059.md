# ISSUE-059 — mcp: item detail output returns file count instead of contents

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: API
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E58`.
[S] The detail formatter discards supplied file paths and contents despite the tool's explicit content promise.

## Reach-and-Impact

[S] A client cannot inspect the promised component source from the detail response.
The review observed formatter output with only a file count; no MCP transport replay was executed.

## Evidence

- [S] `packages/shadcn/src/mcp/index.ts:303-331` promises contents and sends items to the formatter.
- [S] `packages/shadcn/src/mcp/utils.ts:122-141` reduces supplied files to a count.

## Prior-Art

Example dependency omission has a separate promise and formatter in `ISSUE-072`.
Upstream searches remain pending.

## Proposed-Change

Include supplied paths and contents in item details without fabricating absent optional fields.

## Scope-and-Constraints

Distinguish absent content from present empty content.
Do not turn detail viewing into an unsupported transitive dependency fetch.

## API-and-Compatibility

The response must satisfy its advertised detail contract while retaining item metadata.

## Verification

Planned smoke: fetch a known file marker through the detail tool and compare path and content.
Also check optional missing and empty file content.

## Publication-Blockers

A versioned MCP replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce missing MCP source
Action: Invoke item detail on a controlled Registry item containing a unique source marker.
Done-When: Registry payload and exact tool response establish which promised fields are lost.
