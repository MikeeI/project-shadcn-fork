# Issue and Pull Request Tracking

Read this index at the start of every agent session before repository work.
`FORMAT.md` owns research, lifecycle, drafting, implementation, and publication rules.
Each linked `issues/ISSUE-NNN.md` is the complete authoritative record for one root cause.
This file owns `Next finding ID` and projects current issue-file state.
`Next-Action` is the 2–6 word `Next-Action/Summary` projection from the issue record.
When a row disagrees with its issue file, correct the row from the issue file in the same task.

Next finding ID: ISSUE-034

## Open-Findings

| ID | Finding | State | Authorized-Work | Publication-Target | Contribution-Priority | Next-Action | External-Reference |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-001](issues/ISSUE-001.md) | registry: empty-string target bypasses path validation | Investigating | Not-Selected | Not-Selected | High | Reproduce write escape end-to-end | Not published. |
| [ISSUE-002](issues/ISSUE-002.md) | registry: css at-rules load build-executed JavaScript | Investigating | Not-Selected | Not-Selected | High | Verify Tailwind plugin execution locally | Not published. |
| [ISSUE-003](issues/ISSUE-003.md) | chart: ChartStyle interpolates config into a style element unencoded | Investigating | Not-Selected | Not-Selected | High | Verify SSR style breakout | Not published. |
| [ISSUE-004](issues/ISSUE-004.md) | registry: envVars values are written unescaped and without confirmation | Investigating | Not-Selected | Not-Selected | Medium | Confirm env newline injection | Not published. |
| [ISSUE-005](issues/ISSUE-005.md) | registry: middleware rename overwrites an existing proxy.ts unprompted | Investigating | Not-Selected | Not-Selected | High | Reproduce proxy overwrite silently | Not published. |
| [ISSUE-006](issues/ISSUE-006.md) | registry: cssVars values are written into the stylesheet unescaped | Investigating | Not-Selected | Not-Selected | High | Confirm cssVars declaration injection | Not published. |
| [ISSUE-007](issues/ISSUE-007.md) | mcp: failed config reads can overwrite existing client settings | Investigating | Not-Selected | Not-Selected | High | Reproduce MCP config replacement | Not published. |
| [ISSUE-008](issues/ISSUE-008.md) | build: an item named registry collides with catalog output | Investigating | Not-Selected | Not-Selected | High | Reproduce registry output collision | Not published. |
| [ISSUE-009](issues/ISSUE-009.md) | init: no-reinstall still adds an implicit starter button | Investigating | Not-Selected | Not-Selected | High | Reproduce implicit button overwrite | Not published. |
| [ISSUE-010](issues/ISSUE-010.md) | add: dry-run initializes or scaffolds before preview | Investigating | Not-Selected | Not-Selected | High | Reproduce dry-run setup writes | Not published. |
| [ISSUE-011](issues/ISSUE-011.md) | preview: formatting normalization hides semantic literal changes | Investigating | Not-Selected | Not-Selected | Medium | Reproduce hidden literal diff | Not published. |
| [ISSUE-012](issues/ISSUE-012.md) | mcp: total registry failure is returned as successful empty search | Investigating | Not-Selected | Not-Selected | High | Reproduce MCP total failure | Not published. |
| [ISSUE-013](issues/ISSUE-013.md) | registry: declined files enter the final import rewrite | Investigating | Not-Selected | Not-Selected | Medium | Reproduce refused file rewrite | Not published. |
| [ISSUE-014](issues/ISSUE-014.md) | workspace: custom first-file target is reused across groups | Investigating | Not-Selected | Not-Selected | High | Reproduce grouped target collision | Not published. |
| [ISSUE-015](issues/ISSUE-015.md) | registry: workspace comparison conflates different external modules | Investigating | Not-Selected | Not-Selected | Medium | Reproduce external import skip | Not published. |
| [ISSUE-016](issues/ISSUE-016.md) | registry: extension priority lets basename fallback beat exact module path | Investigating | Not-Selected | Not-Selected | Medium | Reproduce exact module displacement | Not published. |
| [ISSUE-017](issues/ISSUE-017.md) | registry: alias rewriting removes legitimate src path segments | Investigating | Not-Selected | Not-Selected | Medium | Reproduce alias suffix corruption | Not published. |
| [ISSUE-018](issues/ISSUE-018.md) | registry: universal content preservation ends before final import rewrite | Investigating | Not-Selected | Not-Selected | Medium | Reproduce universal import mutation | Not published. |
| [ISSUE-019](issues/ISSUE-019.md) | registry: universal TypeScript is renamed to JavaScript without conversion | Investigating | Not-Selected | Not-Selected | Medium | Reproduce universal extension mismatch | Not published. |
| [ISSUE-020](issues/ISSUE-020.md) | registry: empty-string file content is silently omitted | Investigating | Not-Selected | Not-Selected | Medium | Reproduce empty file omission | Not published. |
| [ISSUE-021](issues/ISSUE-021.md) | init: restore removes bak text from a parent directory | Investigating | Not-Selected | Not-Selected | High | Reproduce bak parent recovery | Not published. |
| [ISSUE-022](issues/ISSUE-022.md) | init: explicit base is lost for non-URL component arguments | Investigating | Not-Selected | Not-Selected | Medium | Reproduce non-URL base loss | Not published. |
| [ISSUE-023](issues/ISSUE-023.md) | init: existing Next apps receive new-project CSS cleanup | Investigating | Not-Selected | Not-Selected | High | Reproduce existing Next cleanup | Not published. |
| [ISSUE-024](issues/ISSUE-024.md) | css: Tailwind v3 ignores the preserve-existing-variables option | Investigating | Not-Selected | Not-Selected | Medium | Reproduce v3 variable overwrite | Not published. |
| [ISSUE-025](issues/ISSUE-025.md) | css: unrelated custom variants suppress dark selector registration | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing dark variant | Not published. |
| [ISSUE-026](issues/ISSUE-026.md) | css: prefixed light and dark keys generate double-prefixed references | Investigating | Not-Selected | Not-Selected | Medium | Reproduce prefixed variable reference | Not published. |
| [ISSUE-027](issues/ISSUE-027.md) | fonts: Next Pages reports success without a usable layout integration | Investigating | Not-Selected | Not-Selected | Medium | Reproduce Pages font success | Not published. |
| [ISSUE-028](issues/ISSUE-028.md) | fonts: an existing import suppresses the requested root variable | Investigating | Not-Selected | Not-Selected | Medium | Reproduce imported font omission | Not published. |
| [ISSUE-029](issues/ISSUE-029.md) | fonts: aliased imports produce an unbound generated call | Investigating | Not-Selected | Not-Selected | High | Reproduce aliased font binding | Not published. |
| [ISSUE-030](issues/ISSUE-030.md) | fonts: stylesheet support cache survives later font installation | Investigating | Not-Selected | Not-Selected | Medium | Reproduce stale heading support | Not published. |
| [ISSUE-031](issues/ISSUE-031.md) | tailwind: shorthand content prevents configuration discovery | Investigating | Not-Selected | Not-Selected | Medium | Reproduce shorthand config omission | Not published. |
| [ISSUE-032](issues/ISSUE-032.md) | scaffold: React Router monorepo retains the template font | Investigating | Not-Selected | Not-Selected | Medium | Reproduce scaffold font mismatch | Not published. |
| [ISSUE-033](issues/ISSUE-033.md) | scaffold: canceled project prompts fall through to defaults | Investigating | Not-Selected | Not-Selected | Medium | Reproduce canceled project creation | Not published. |

## Archived-Findings

| ID | Finding | Authorized-Work | Publication-Target | Contribution-Priority | Archive-Reason | External-Reference |
| --- | --- | --- | --- | --- | --- | --- |
