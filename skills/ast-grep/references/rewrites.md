# Rewrites

Read this before applying syntax rewrites, especially when a rule uses captures or transforms.

## Preview before applying

A YAML `fix` reuses captures from `rule`. This example renames only `old_api` calls and carries their arguments through. Save it as `rename-old-api.yml`:

```yaml
id: rename-old-api
language: rust
rule:
  pattern: old_api($$$ARGS)
fix: new_api($$$ARGS)
```

Run `ast-grep scan --rule rename-old-api.yml calls.rs` to preview a diff. Scanning without `--update-all` leaves the file unchanged. Use `--interactive` to select proposed edits or `--update-all` to apply every match:

```sh
ast-grep scan --rule rename-old-api.yml --update-all calls.rs
```

Check the resulting diff and confirm the rule matched only intended syntax. A fix replaces the whole matched node, so line breaks within it may change. Check for remaining old forms and run the relevant formatter, compiler, or tests when appropriate. A structural match does not guarantee that replacing the node preserves program behavior.

For a one-off rewrite, `run` accepts the same capture reuse in `--rewrite`:

```sh
ast-grep run --lang rust --pattern 'old_api($$$ARGS)' --rewrite 'new_api($$$ARGS)' src/
```

This previews the change. Add `--update-all` to apply every match or use `--interactive` to choose edits.

## Transform captured text

When a fix needs derived text rather than the original capture, define a named value under `transform` and use it in `fix`. This rule changes a `debug_` API prefix to `release_` while preserving arguments:

```yaml
id: rename-debug-api
language: rust
rule:
  pattern: $NAME($$$ARGS)
constraints:
  NAME:
    regex: '^debug_'
transform:
  NEW_NAME:
    replace:
      source: $NAME
      replace: '^debug_'
      by: 'release_'
fix: $NEW_NAME($$$ARGS)
```

`transform` can also convert case or take a substring. Keep a transform only when the replacement needs that change; otherwise reuse the original metavariable directly in `fix`. Preview and review transformed fixes the same way as other rewrites.
