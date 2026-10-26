---
name: outline
description: Use when you need a structural map of declarations, imports, exports, or direct members in known source files or directories.
---

# Outline

Use `ast-grep outline` when you know the likely file or directory and need a syntax-level map to choose source ranges to read. For behavior searches with unknown locations, use [semble](../semble/SKILL.md) first, then outline promising files when it helps. Use [ast-grep](../ast-grep/SKILL.md) for structural code-pattern matches.

Check that the installed CLI supports the subcommand with `ast-grep outline --help`. If it does not, inspect the source directly and use available search tools. Do not assume a newer binary or silently treat unavailable output as an empty result.

Start with the narrowest useful path. A file defaults to local declarations with `--items structure` and a digest of direct members. A directory defaults to exported items with `--items exports`, grouped by name. If any path argument is a directory, the directory defaults apply to the whole command, including when file paths are mixed in. Set the filters explicitly when that is not what you need:

```bash
ast-grep outline src/parser.ts
ast-grep outline src/parser.ts --items imports --view signatures
ast-grep outline src/ --items exports --type class,function
ast-grep outline src/parser.ts --match Parser --type class --view expanded
ast-grep outline src/parser.ts --match Parser --type class --view expanded --pub-members
```

`--items` selects top-level entries: `structure` for locally defined items, `imports`, `exports`, or `all` for all top-level entries. `--view` controls detail: `names` groups names, `signatures` shows one line per top-level item, `digest` adds compact direct member names, and `expanded` gives direct members their own source lines. Use `--match` for a case-sensitive Rust regex over top-level names/signatures; it does not filter members. `--type` filters top-level symbol types such as `class,function`, not member types. `--pub-members` filters the members shown in digest or expanded views.

Use `--json=stream` only when a script needs structured entries or source ranges. It emits one JSON file object per line. Prefer text output for navigation. Read the indicated source before making claims about implementation details, and do not run outline repeatedly when the existing map already answers which range to inspect.

Outline reports syntax, not program meaning. It does not resolve references or re-export chains, infer types, or construct a call graph. An absent entry does not prove that a symbol or behavior is absent; parser coverage, language rules, or project configuration can affect extraction. Use text/reference search or compiler-aware tools for resolution questions, then inspect the source.
