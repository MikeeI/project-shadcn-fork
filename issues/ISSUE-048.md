# ISSUE-048 — search: generated GitHub JSON addresses fail their consumer roundtrip

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

Review reference: `E47`, using explicit HEAD rather than an extra ref-resolution request.
[S] Search generates an unreferenced GitHub JSON item address that its own consumer parses as a local file.

## Reach-and-Impact

[S] Search recommendations and the MCP example-loading path can address the wrong source.
The review observed file classification without a ref and GitHub classification with `#HEAD` or an explicit tag.
No GitHub-fetch roundtrip was executed.

## Evidence

- [S] `packages/registry/src/registry/search.ts:317-324` omits a ref when the input did not specify one.
- [S] `packages/registry/src/registry/github-ref.ts:35,116-118` already defines the implicit default as HEAD.
- [S] `packages/shadcn/src/mcp/index.ts:379-383` consumes generated search arguments for item loading.

## Prior-Art

`ISSUE-047` concerns explicit namespaces; correcting only those does not disambiguate GitHub addresses.
Upstream searches remain pending.

## Proposed-Change

Emit `#HEAD` for unreferenced GitHub JSON items that would otherwise classify locally, retaining explicit refs.

## Scope-and-Constraints

Do not invent `main`, fetch a new ref unnecessarily or change ordinary local JSON interpretation.
Preserve explicit branches, tags and SHAs.

## API-and-Compatibility

Generated add arguments must retain their remote identity when consumed by the same API.

## Verification

Planned smoke: generate and parse JSON-item arguments with implicit HEAD and explicit tags.
Then exercise Search-to-item lookup against a controlled available registry.

## Publication-Blockers

A versioned public Search-to-fetch replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce GitHub search roundtrip
Action: Search a GitHub registry for a JSON-named item and feed its add argument into item lookup.
Done-When: Generated argument, parsed scheme and actual source fetch are captured.
