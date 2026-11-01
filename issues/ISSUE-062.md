# ISSUE-062 — proxy: empty destination ports become zero in SOCKS connections

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

Review reference: `E61`.
[S] The custom SOCKS connector converts Undici's empty URL port with `Number`, producing destination port zero.

## Reach-and-Impact

[S] Ordinary HTTP/HTTPS addresses without a nonstandard port can fail through SOCKS before reaching the service.
Explicit standard ports are also normalized away by URL parsing; no proxy replay was executed.

## Evidence

- [S] `packages/registry/src/registry/proxy.ts:44-56` passes the converted target port into SOCKS.
- [S] Undici 7.30.1 passes URL port to custom connectors: https://raw.githubusercontent.com/nodejs/undici/v7.30.1/lib/dispatcher/client.js
- [S] Default ports are added only by its native connector: https://raw.githubusercontent.com/nodejs/undici/v7.30.1/lib/core/connect.js

## Prior-Art

TLS and bypass handling have independent causes in `ISSUE-063` and `ISSUE-064`.
Upstream searches remain pending.

## Proposed-Change

Replace only an empty target port with protocol-appropriate 80 or 443 before SOCKS connection setup.

## Scope-and-Constraints

Keep an explicit `:0` distinct from a missing port and preserve nonstandard ports.
The proxy server's own port is not the destination default.

## API-and-Compatibility

SOCKS transport must preserve the ordinary URL destination semantics of direct HTTP/HTTPS.

## Verification

Planned smoke: capture actual SOCKS CONNECT ports for HTTP and HTTPS with omitted ports.
Use HTTP to isolate this issue from the separate missing TLS upgrade.

## Publication-Blockers

A versioned real SOCKS replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce SOCKS default ports
Action: Fetch an HTTP Registry URL without an explicit port through a controlled SOCKS endpoint.
Done-When: Requested URL, connector options and actual CONNECT destination port are recorded.
