# ISSUE-075 — github: raw file buffer overflow is reported as a network failure

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-11-01
Updated: 2026-11-01
Source: `upstream/main@c0d63c339d6cd2fc26135590a54cf1f362269451`

## Root-Cause

Review reference: `E74`, limited to a proven Raw-file stdout buffer overflow.
[S] The gh transport maps an Execa buffer failure to network before the later byte-size check can run.

## Reach-and-Impact

[S] An oversized ASCII source fetched through gh can receive misleading network guidance instead of oversize guidance.
The size restriction itself is not bypassed; no gh download was executed.

## Evidence

- [S] `packages/registry/src/registry/github-cli.ts:193-270` shares error classification and applies maxBuffer before size checking.
- [S] Execa 9.6.0 error properties: https://raw.githubusercontent.com/sindresorhus/execa/v9.6.0/docs/errors.md
- [S] Stream/character-limit semantics: https://raw.githubusercontent.com/sindresorhus/execa/v9.6.0/docs/output.md

## Prior-Art

No duplicate cause exists in the local ledger.
Upstream issues, PRs, discussions and releases remain unsearched for this finding.

## Proposed-Change

Classify a proven raw-file stdout overflow as oversize while preserving distinct stderr and Ref-response failures.

## Scope-and-Constraints

Do not globally map every isMaxBuffer result to file oversize.
Use an ASCII case to align character and byte thresholds and preserve existing transport limits.

## API-and-Compatibility

A diagnostic correction must not mislabel another operation merely because it shares runGhApi.

## Verification

Planned smoke: fetch a raw ASCII file above the 5 MiB limit through the actual gh path.
Check oversize guidance, with stderr and Ref-output overflows as negative controls.

## Publication-Blockers

A versioned gh replay, reliable overflowing-stream identification and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce gh oversize classification
Action: Fetch an oversized controlled source through the existing authenticated gh fallback path.
Done-When: gh/Execa versions, overflow stream, error classification and displayed guidance are captured.
