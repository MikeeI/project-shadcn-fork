# ISSUE-061 — mcp: generated add commands do not quote item arguments

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

Review reference: `E60`.
[S] MCP command output joins raw item arguments with spaces instead of serializing their shell boundaries.

## Reach-and-Impact

[S] Copying a URL with `&` into a POSIX shell changes the executed command and truncates its URL argument.
The server only emits text; it does not execute that command itself.
No shell replay was executed.

## Evidence

- [S] `packages/shadcn/src/mcp/utils.ts:55` emits a raw add argument.
- [S] `packages/shadcn/src/mcp/index.ts:390-403` joins multiple raw items.
- [S] Existing POSIX convention: `packages/shadcn/src/utils/handle-error.ts:105-110`.

## Prior-Art

Wrong package-manager runner choice has a separate UI owner in `ISSUE-069`.
Upstream searches remain pending.

## Proposed-Change

Quote each item under an explicit POSIX-shell contract before combining arguments in both output paths.

## Scope-and-Constraints

Preserve multiple distinct items, apostrophes and other argument characters.
Do not claim POSIX quoting is automatically correct for PowerShell or cmd.exe.

## API-and-Compatibility

A suggested command must reproduce the original item argument list when executed in the supported shell.

## Verification

Planned smoke: evaluate both outputs with an argument-recording process instead of an installer.
Compare query ampersands, apostrophes, spaces and two separate items byte-for-byte.

## Publication-Blockers

A versioned shell replay, explicit shell contract and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce MCP shell arguments
Action: Pass a generated multi-query URL command to a harmless argument-recording process in a POSIX shell.
Done-When: Original items and actual argv demonstrate whether argument boundaries survive.
