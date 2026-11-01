# Issue and Pull Request Tracking

Read this index at the start of every agent session before repository work.
`FORMAT.md` owns research, lifecycle, drafting, implementation, and publication rules.
Each linked `issues/ISSUE-NNN.md` is the complete authoritative record for one root cause.
This file owns `Next finding ID` and projects current issue-file state.
`Next-Action` is the 2–6 word `Next-Action/Summary` projection from the issue record.
When a row disagrees with its issue file, correct the row from the issue file in the same task.

Next finding ID: ISSUE-076

## Open-Findings

| ID | Finding | State | Authorized-Work | Publication-Target | Contribution-Priority | Next-Action | External-Reference |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-001](issues/ISSUE-001.md) | registry: empty-string target bypasses path validation | Investigating | Not-Selected | Not-Selected | High | Reproduce write escape end-to-end | Not published. |
| [ISSUE-002](issues/ISSUE-002.md) | registry: css at-rules load build-executed JavaScript | Investigating | Not-Selected | Not-Selected | High | Verify Tailwind plugin execution locally | Not published. |
| [ISSUE-003](issues/ISSUE-003.md) | chart: ChartStyle interpolates config into a style element unencoded | Investigating | Not-Selected | Not-Selected | High | Verify SSR style breakout | Not published. |
| [ISSUE-004](issues/ISSUE-004.md) | registry: envVars values are written unescaped and without confirmation | Investigating | Not-Selected | Not-Selected | Medium | Reproduce env value roundtrip | Not published. |
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
| [ISSUE-034](issues/ISSUE-034.md) | dependencies: Expo receives packages after the passthrough separator | Investigating | Not-Selected | Not-Selected | Medium | Reproduce Expo argument handling | Not published. |
| [ISSUE-035](issues/ISSUE-035.md) | dependencies: dev-only presence suppresses a runtime requirement | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing runtime promotion | Not published. |
| [ISSUE-036](issues/ISSUE-036.md) | dependencies: overlapping Dev requests demote required runtime packages | Investigating | Not-Selected | Not-Selected | Medium | Reproduce dependency category demotion | Not published. |
| [ISSUE-037](issues/ISSUE-037.md) | scaffold: fallback replaces a successfully detected package manager | Investigating | Not-Selected | Not-Selected | Medium | Reproduce detected manager replacement | Not published. |
| [ISSUE-038](issues/ISSUE-038.md) | workspace: application envVars are written into the UI package | Investigating | Not-Selected | Not-Selected | Medium | Reproduce application env misrouting | Not published. |
| [ISSUE-039](issues/ISSUE-039.md) | env: exported existing keys are duplicated during merge | Investigating | Not-Selected | Not-Selected | Medium | Reproduce exported key overwrite | Not published. |
| [ISSUE-040](issues/ISSUE-040.md) | env: merging removes quotes required to preserve new values | Investigating | Not-Selected | Not-Selected | Medium | Reproduce env quote loss | Not published. |
| [ISSUE-041](issues/ISSUE-041.md) | resolver: descendants reverse declared sibling override priority | Investigating | Not-Selected | Not-Selected | Medium | Reproduce sibling override reversal | Not published. |
| [ISSUE-042](issues/ISSUE-042.md) | resolver: Theme-first resort reverses a real dependency edge | Investigating | Not-Selected | Not-Selected | Medium | Reproduce Theme edge reversal | Not published. |
| [ISSUE-043](issues/ISSUE-043.md) | registry: namespace discovery stops before newly mapped dependencies | Investigating | Not-Selected | Not-Selected | Medium | Reproduce transitive namespace omission | Not published. |
| [ISSUE-044](issues/ISSUE-044.md) | registry: direct catalogs bypass duplicate item-name validation | Investigating | Not-Selected | Not-Selected | Medium | Reproduce direct catalog duplicates | Not published. |
| [ISSUE-045](issues/ISSUE-045.md) | registry: directory lookup precedes an already-configured skip | Investigating | Not-Selected | Not-Selected | Medium | Reproduce configured registry failure | Not published. |
| [ISSUE-046](issues/ISSUE-046.md) | registry: accepted namespace names cannot be resolved later | Investigating | Not-Selected | Not-Selected | Medium | Reproduce namespace grammar mismatch | Not published. |
| [ISSUE-047](issues/ISSUE-047.md) | registry: explicit namespace JSON items are classified as local files | Investigating | Not-Selected | Not-Selected | Medium | Reproduce namespaced JSON dispatch | Not published. |
| [ISSUE-048](issues/ISSUE-048.md) | search: generated GitHub JSON addresses fail their consumer roundtrip | Investigating | Not-Selected | Not-Selected | Medium | Reproduce GitHub search roundtrip | Not published. |
| [ISSUE-049](issues/ISSUE-049.md) | preview: custom path is omitted from file and import planning | Investigating | Not-Selected | Not-Selected | Medium | Reproduce custom path preview | Not published. |
| [ISSUE-050](issues/ISSUE-050.md) | preview: bare diff and view overviews omit CSS changes | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing CSS overview | Not published. |
| [ISSUE-051](issues/ISSUE-051.md) | preview: CSS transformation uses different context and overwrite policy | Investigating | Not-Selected | Not-Selected | Medium | Reproduce CSS preview divergence | Not published. |
| [ISSUE-052](issues/ISSUE-052.md) | preview: workspace target configuration is not resolved | Investigating | Not-Selected | Not-Selected | Medium | Reproduce workspace preview target | Not published. |
| [ISSUE-053](issues/ISSUE-053.md) | preview: Env files ignore alternative targets and append-only merge | Investigating | Not-Selected | Not-Selected | Medium | Reproduce Env file preview | Not published. |
| [ISSUE-054](issues/ISSUE-054.md) | preview: applicable Tailwind v3 configuration changes are absent | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing Tailwind preview | Not published. |
| [ISSUE-055](issues/ISSUE-055.md) | preview: Next 16 middleware lacks proxy target and export conversion | Investigating | Not-Selected | Not-Selected | Medium | Reproduce proxy preview mismatch | Not published. |
| [ISSUE-056](issues/ISSUE-056.md) | preview: missing required CSS is incorrectly planned as creation | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing CSS promise | Not published. |
| [ISSUE-057](issues/ISSUE-057.md) | mcp: cwd option is not forwarded to tool instances | Investigating | Not-Selected | Not-Selected | Medium | Reproduce MCP cwd loss | Not published. |
| [ISSUE-058](issues/ISSUE-058.md) | mcp: project style and Tailwind normalization context are lost | Investigating | Not-Selected | Not-Selected | Medium | Reproduce MCP style fallback | Not published. |
| [ISSUE-059](issues/ISSUE-059.md) | mcp: item detail output returns file count instead of contents | Investigating | Not-Selected | Not-Selected | Medium | Reproduce missing MCP source | Not published. |
| [ISSUE-060](issues/ISSUE-060.md) | mcp: example search hides partial registry failures | Investigating | Not-Selected | Not-Selected | Medium | Reproduce hidden example failure | Not published. |
| [ISSUE-061](issues/ISSUE-061.md) | mcp: generated add commands do not quote item arguments | Investigating | Not-Selected | Not-Selected | Medium | Reproduce MCP shell arguments | Not published. |
| [ISSUE-062](issues/ISSUE-062.md) | proxy: empty destination ports become zero in SOCKS connections | Investigating | Not-Selected | Not-Selected | Medium | Reproduce SOCKS default ports | Not published. |
| [ISSUE-063](issues/ISSUE-063.md) | proxy: HTTPS over SOCKS returns a raw TCP socket | Investigating | Not-Selected | Not-Selected | Medium | Reproduce SOCKS HTTPS failure | Not published. |
| [ISSUE-064](issues/ISSUE-064.md) | proxy: SOCKS dispatch ignores documented NO_PROXY exceptions | Investigating | Not-Selected | Not-Selected | Medium | Reproduce SOCKS bypass omission | Not published. |
| [ISSUE-065](issues/ISSUE-065.md) | resolver: cache bypass is lost in the Base-Color helper | Investigating | Not-Selected | Not-Selected | Medium | Reproduce stale Base Color | Not published. |
| [ISSUE-066](issues/ISSUE-066.md) | registry: URL normalization disconnects configured request headers | Investigating | Not-Selected | Not-Selected | Medium | Reproduce normalized header loss | Not published. |
| [ISSUE-067](issues/ISSUE-067.md) | registry: malformed JSON error bodies hide typed HTTP failures | Investigating | Not-Selected | Not-Selected | Medium | Reproduce malformed error masking | Not published. |
| [ISSUE-068](issues/ISSUE-068.md) | command menu: global copy handles stale state after dialog close | Investigating | Not-Selected | Not-Selected | Medium | Reproduce stale menu copy | Not published. |
| [ISSUE-069](issues/ISSUE-069.md) | command menu: npm preference generates unsupported npm dlx | Investigating | Not-Selected | Not-Selected | Medium | Reproduce npm menu command | Not published. |
| [ISSUE-070](issues/ISSUE-070.md) | command menu: documentation guides are assumed installable Registry items | Investigating | Not-Selected | Not-Selected | Medium | Reproduce guide install recommendation | Not published. |
| [ISSUE-071](issues/ISSUE-071.md) | command block: copy indicator succeeds before clipboard result | Investigating | Not-Selected | Not-Selected | Medium | Reproduce false copy success | Not published. |
| [ISSUE-072](issues/ISSUE-072.md) | mcp: example output omits declared dependency categories | Investigating | Not-Selected | Not-Selected | Low | Reproduce missing example dependencies | Not published. |
| [ISSUE-073](issues/ISSUE-073.md) | registry: appended query parameters land after URL fragments | Investigating | Not-Selected | Not-Selected | Low | Reproduce fragment query loss | Not published. |
| [ISSUE-074](issues/ISSUE-074.md) | registry: generic HTTP errors hide supplied server messages in CLI output | Investigating | Not-Selected | Not-Selected | Low | Reproduce hidden server detail | Not published. |
| [ISSUE-075](issues/ISSUE-075.md) | github: raw file buffer overflow is reported as a network failure | Investigating | Not-Selected | Not-Selected | Low | Reproduce gh oversize classification | Not published. |

## Archived-Findings

| ID | Finding | Authorized-Work | Publication-Target | Contribution-Priority | Archive-Reason | External-Reference |
| --- | --- | --- | --- | --- | --- | --- |
