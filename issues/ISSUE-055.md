# ISSUE-055 — preview: Next 16 middleware lacks proxy target and export conversion

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

Review reference: `E54`.
[S] Preview omits the installer's Next-specific effective filename and source-export transformation.

## Reach-and-Impact

[S] Recognized Next >=16 nonuniversal root middleware is previewed under the wrong name and compared to wrong content.
The predicate does not cover arbitrary src middleware or unrecognized version labels.
No CLI parity replay was executed.

## Evidence

- [S] `packages/shadcn/src/utils/dry-run.ts:167-218` lacks the Next preparation.
- [S] `packages/registry/src/updaters/update-files.ts:192-194,258-260,831-851` transforms and renames actual output.

## Prior-Art

The actual overwrite-check ordering defect is separately owned by `ISSUE-005`.
Upstream searches remain pending.

## Proposed-Change

Apply the same effective Next destination and export transformation in preview planning.

## Scope-and-Constraints

Preserve detected-version/root-file gates and universal-content exemptions.
Correcting preview alone does not fix the actual installer overwrite guard.

## API-and-Compatibility

Preview must describe the actual `proxy` file and export produced by the supported Next conversion.

## Verification

Planned smoke: preview nonuniversal root middleware under Next 16 with and without existing proxy output.
Compare filename, export and conflict decision to effective installation planning.

## Publication-Blockers

A versioned Next CLI replay and upstream prior-art search remain pending.

## Next-Action

Summary: Reproduce proxy preview mismatch
Action: Preview a root middleware item in an isolated recognized Next 16 project.
Done-When: Preview path/export and actual prepared target/export are captured.
