# ISSUE-067 — registry: malformed JSON error bodies hide typed HTTP failures

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

Review reference: `E66`.
[S] JSON body parsing can throw before a non-success HTTP response reaches its typed status-error branch.

## Reach-and-Impact

[S] A 401 with empty or truncated JSON yields a parse error instead of the documented unauthorized error.
No HTTP replay was executed; successful malformed JSON must still fail normally.

## Evidence

- [S] `packages/registry/src/registry/fetcher.ts:59-103` parses before classifying status.
- [S] `packages/registry/README.md:158-194` documents typed Registry errors.

## Prior-Art

Valid custom-message visibility is independently tracked as `ISSUE-074`.
Upstream searches remain pending.

## Proposed-Change

Parse non-success response details best effort, then classify by HTTP status even when error-body parsing fails.

## Scope-and-Constraints

Do not suppress successful-response parse errors or unrelated transport failures.
Keep existing typed status classes, codes and suggestions.

## API-and-Compatibility

Malformed optional error detail must not replace the primary public HTTP error contract.

## Verification

Planned smoke: serve empty, truncated and valid JSON error bodies with status 401.
Unauthorized class, code and status must survive all three.

## Publication-Blockers

A versioned public HTTP/API replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce malformed error masking
Action: Fetch a controlled 401 response labeled JSON with an empty or truncated body.
Done-When: Response metadata and exact thrown Registry class/code/status are captured.
