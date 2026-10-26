---
name: ast-grep
description: Use when you need syntax-aware code searches, YAML rules, or AST-based rewrites where exact structure matters.
---

# ast-grep

Use this skill for searches and edits that depend on parsed code shape, such as a call with particular arguments or a node inside a specific scope. Syntax matches do not prove runtime behavior, type identity, data flow, or semantic equivalence.

Set `--lang` or YAML `language` for the code being scanned. Concrete examples use Rust; adapt patterns, surrounding context, and node kinds to the target grammar.

## Choose a command

- Use `ast-grep run --lang LANG --pattern 'PATTERN' PATH...` for a quick syntax pattern. Quote patterns containing `$` so the shell preserves metavariables.
- Use `ast-grep scan --rule RULE.yml PATH...` for a reusable YAML rule with constraints, relationships, captures, or fixes.
- For pattern construction, scope boundaries, JSON captures, and maintained rule tests, read [references/rules.md](references/rules.md).
- Before applying a rewrite or using a YAML fix and transforms, read [references/rewrites.md](references/rewrites.md).

Use [semble](../semble/SKILL.md) for meaning-based discovery and [outline](../outline/SKILL.md) for source inventories.
