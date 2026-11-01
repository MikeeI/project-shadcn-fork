# ISSUE-064 — proxy: SOCKS dispatch ignores documented NO_PROXY exceptions

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E63`.
[S] Selecting a SOCKS ALL_PROXY returns a single agent before any target-specific NO_PROXY decision.

## Reach-and-Impact

[S] A local Registry can be contacted from the remote proxy's loopback instead of directly.
This is not a proven precedence claim about every protocol-specific proxy variable.
No proxy replay was executed.

## Evidence

- [S] `packages/registry/src/registry/proxy.ts:61-77,134-153` uses the selected dispatcher for requests and redirects.
- [S] `packages/registry/README.md:196-205` documents the proxy environment-variable contract.

## Prior-Art

Port defaults and TLS have separate owners in `ISSUE-062` and `ISSUE-063`.
Upstream searches remain pending.

## Proposed-Change

Honor documented NO_PROXY matching for each actual target before SOCKS, including redirect targets.

## Scope-and-Constraints

Retain host, port and wildcard semantics and ordinary external proxy use.
Do not invent unsupported protocol-variable precedence or runtime Env reload promises.

## API-and-Compatibility

A configured bypass must remain a bypass regardless of the chosen supported proxy transport.

## Verification

Planned smoke: access local bypass and external proxied targets, then redirect to a bypass target.
Only nonexcluded targets may produce SOCKS CONNECT requests.

## Publication-Blockers

A versioned routing replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce SOCKS bypass omission
Action: Fetch a local HTTP Registry with SOCKS ALL_PROXY and NO_PROXY selecting localhost.
Done-When: Direct endpoint access and proxy CONNECT logs establish the actual route.
