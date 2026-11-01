# ISSUE-022 — init: explicit base is lost for non-URL component arguments

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

Review reference: `E19`.
[S] Explicit `--base` is propagated through URL rewriting but not the ordinary first-component path.

## Reach-and-Impact

[S] `init button --base aria` can use an unrelated default when no matching base config or preset exists.
The flag is not ineffective in every init path; no remote installation was executed.

## Evidence

- [S] `packages/shadcn/src/commands/init.ts:478-519` computes base precedence and only rewrites URL input.
- [S] Later config resolution at `:565-585,764-767` does not receive that explicit choice.

## Prior-Art

No matching root exists in the local ledger.
Upstream issues, PRs, discussions and releases remain unsearched for this finding.

## Proposed-Change

Carry the explicit library selection into the actual Registry/style context before resolving ordinary components.

## Scope-and-Constraints

Preserve explicit preset/style precedence and already working URL paths.
Do not replace a meaningful style choice with a fresh generic default.

## API-and-Compatibility

The documented explicit base flag must control both source resolution and resulting configuration.

## Verification

Planned smoke: initialize an ordinary component with `--base aria`, without preset or defaults.
Compare config and imports; retain URL and preset controls.

## Publication-Blockers

A versioned CLI replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce non-URL base loss
Action: Initialize a supported throwaway project with a positional component and explicit alternate base.
Done-When: Invocation, resolved URLs, generated config and imports show the actual selected library.
