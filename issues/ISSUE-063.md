# ISSUE-063 — proxy: HTTPS over SOCKS returns a raw TCP socket

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

Review reference: `E62`.
[S] The custom connector returns the SOCKS TCP tunnel for HTTPS without performing the TLS upgrade it replaces.

## Reach-and-Impact

[S] An otherwise reachable HTTPS Registry fails because Undici does not add TLS after this custom callback.
No HTTPS/SOCKS request was executed during review.

## Evidence

- [S] `packages/registry/src/registry/proxy.ts:44-56` returns the SOCKS socket directly.
- [S] Undici connector dispatch: https://raw.githubusercontent.com/nodejs/undici/v7.30.1/lib/dispatcher/client.js
- [S] Native TLS and httpSocket handling: https://raw.githubusercontent.com/nodejs/undici/v7.30.1/lib/core/connect.js

## Prior-Art

Destination-port handling is separately tracked as `ISSUE-062`.
Upstream thread and release searches remain pending.

## Proposed-Change

Upgrade HTTPS tunnels using native-compatible TLS before callback success, with correct server name and error cleanup.

## Scope-and-Constraints

Keep HTTP raw and report connection failure exactly once.
Preserve native certificate validation and do not bypass verification to make a smoke pass.

## API-and-Compatibility

A custom connector owns the complete HTTPS connection contract, not merely the TCP tunnel.

## Verification

Planned smoke: fetch a real HTTPS JSON endpoint on an explicit nonstandard port through SOCKS.
Require a successful handshake and response to isolate the default-port defect.

## Publication-Blockers

A versioned HTTPS/SOCKS replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce SOCKS HTTPS failure
Action: Fetch a controlled HTTPS Registry endpoint with explicit nonstandard port through SOCKS.
Done-When: Tunnel establishment, TLS handshake outcome and HTTP result are captured.
