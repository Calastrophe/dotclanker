# Search rules

Read this when a one-node search needs filters, scope, reusable logic, machine-readable captures, or tests.

## Start with a syntax pattern

`run` is useful for a quick search:

```sh
ast-grep run --lang rust --pattern '$RESULT.unwrap()' src/
```

The single quotes keep `$RESULT` from shell expansion. `$NAME` captures one syntax node. `$$$ITEMS` captures zero or more nodes in a list, such as call arguments or statements. A repeated metavariable requires the same captured syntax at each occurrence. A pattern must parse as code in the selected language; it matches syntax, not text inside a string or comment.

Use a pattern object when a fragment is incomplete or ambiguous. `context` supplies parseable surrounding code, and `selector` names the syntax node within that context that should match:

```yaml
rule:
  pattern:
    context: |
      fn parse($ARG: &str) {}
    selector: parameter
```

This matches a Rust parameter such as `name: &str`. To inspect pattern parsing, use `--debug-query=pattern`. To see node kinds in a representative snippet, pass that snippet as `--pattern` with `--debug-query=ast`; the output describes the pattern snippet, not every file being scanned.

## Match calls in one Rust function

This rule finds `unwrap()` calls whose receiver is a single identifier inside `load_config`. It stops at nested function items and closures. Save it as `unwrap-in-load-config.yml`:

```yaml
id: unwrap-in-load-config
language: rust
rule:
  pattern: $RESULT.unwrap()
  inside:
    pattern: |
      fn load_config() {
        $$$BODY
      }
    stopBy:
      any:
        - kind: function_item
        - kind: closure_expression
constraints:
  RESULT:
    kind: identifier
```

The example matches only the direct call below. The member receiver, closure call, nested function call, string contents, and call in another function do not match:

```rust
struct Config { result: Result<(), ()> }

fn load_config() {
    let result: Result<(), ()> = Ok(());
    let config = Config { result: Ok(()) };
    let direct = result.unwrap();                 // match
    let field = config.result.unwrap();            // constraint rejects field receiver
    let closure = || result.unwrap();              // stops at closure
    fn helper() {                                  // stops at nested function item
        let result: Result<(), ()> = Ok(());
        result.unwrap();
    }
    let text = "result.unwrap()";                 // string, not a call node
}

fn elsewhere() {
    let result: Result<(), ()> = Ok(());
    result.unwrap();                              // outside load_config
}
```

Run the rule against a Rust file with `ast-grep scan --rule unwrap-in-load-config.yml fixture.rs`. The outer pattern describes a function with no parameters, modifiers, or explicit return type. Change it to match the signature present in the source. The tree-sitter parse does not check whether a name resolves to `Result`, `Option`, or another type.

`inside` searches ancestors. Its default `stopBy: neighbor` checks only the parent; `stopBy: end` keeps searching to the root. A custom stop rule checks each ancestor and stops at the first match. The boundary is inclusive, so it can also satisfy the relation. Here the nearest `function_item` or `closure_expression` ends the search. A nested function therefore cannot inherit the outer function's match.

`has` searches children, while `follows` and `precedes` search siblings in one direction. Their default is also `neighbor`; use `end` or a custom stop rule when the relation should search farther. These relations describe tree position only.

## Compose rules and filter captures

Atomic rules select a `pattern`, `kind`, `regex`, `nthChild`, or `range`. A `regex` tests the full text of each candidate node, including its children, so pair it with a syntax rule when possible.

Composite rules combine checks on the same candidate node. `all` requires each subrule, `any` requires one, and `not` excludes a subrule. `matches` applies a named reusable rule from the YAML file's `utils` mapping:

```yaml
utils:
  unwrap-call:
    pattern: $RESULT.unwrap()
rule:
  matches: unwrap-call
```

Use `constraints` to filter a captured metavariable after the main rule matches. The scoped Rust rule above requires `RESULT` to be an identifier. To filter captured text instead, use `regex: '^result$'` under `RESULT`. Constraints apply to single metavariables, not `$$$` captures.

## Try a rule on stdin

For a one-node check, pipe a small snippet to `run`:

```sh
printf '%s\n' 'fn load_config() { let value = result.unwrap(); }' |
  ast-grep run --lang rust --pattern '$RESULT.unwrap()' --stdin
```

For a YAML rule check without a file, pass the rule as a single-quoted multiline argument. The YAML double quotes preserve literal metavariables:

```sh
printf '%s\n' 'fn load_config() { let value = result.unwrap(); }' |
  ast-grep scan --inline-rules 'id: unwrap-call
language: rust
rule:
  pattern: "$RESULT.unwrap()"' --stdin
```

## Read JSON matches

`--json` emits an array. `--json=stream` emits one JSON object per line. Current output exposes the matched `.text`, `.file`, `.range`, and captures under `.metaVariables.single` or `.metaVariables.multi`. For example:

```sh
ast-grep scan --rule unwrap-in-load-config.yml --json=stream fixture.rs |
  jq -r '"\(.file):\(.range.start.line + 1): \(.metaVariables.single.RESULT.text)"'
```

This prints the file, one-based line, and captured receiver. JSON range line and column values are zero-based. Each `$$$` capture appears as a list under `metaVariables.multi`.

## Diagnose and test rules

If a scan finds nothing, check the path, language, extension, and ignored-file settings first. Try the pattern on a known snippet, then inspect its parse with `--debug-query=pattern` or `--debug-query=ast`. Keep the scan path narrow while iterating. Parsers may not expand preprocessing constructs or resolve imports and qualified paths. For example, Rust macros are not expanded; write patterns for the syntax actually present in source.

For a maintained rule, configure `sgconfig.yml` with its rule directory and test directory:

```yaml
ruleDirs:
  - rules
testConfigs:
  - testDir: rule-tests
```

Add a file such as `rule-tests/unwrap-in-load-config-test.yml` with the matching rule id and `valid`/`invalid` source snippets:

```yaml
id: unwrap-in-load-config
valid:
  - |
    fn load_config() {
      let closure = || result.unwrap();
      fn helper() { result.unwrap(); }
    }
  - |
    fn elsewhere() { result.unwrap(); }
invalid:
  - |
    fn load_config() { result.unwrap(); }
```

Run `ast-grep test --skip-snapshot-tests` to check which snippets match. After the rule output is stable, `ast-grep test --update-all` records snapshots; run `ast-grep test` afterward to check both cases and snapshot output.
