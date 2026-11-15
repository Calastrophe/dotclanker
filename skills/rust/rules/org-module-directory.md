# org-module-directory

> Group sibling files that share one concept under a module directory, and keep the module's main type in the module file.

## Why it matters

Prefixed sibling files repeat the concept in every name and hide that they belong together. A module directory states the grouping once, and a main type in the module file means `budget.rs` is the one place to find `Budget`.

## Guidance

When sibling files share a name prefix and one concept, move them under a module directory named for that concept and drop the redundant prefix from the file names.

Define the module's main type in the module file itself, next to the `mod` declarations and re-exports. Only the secondary parts live under the directory. Do not make the module file declarations-only and push the main type into an invented leaf such as `limit.rs`, `record.rs` or `policy.rs` to avoid `clippy::module_inception`.

Use `name.rs` with a `name/` directory; do not use `mod.rs`.

Boundaries:

- Do not create a directory for a single file.
- Do not group files that share a word but not a concept.
- A module with no single main type, such as a crate or feature root that only gathers submodules, or a feature split into `api.rs` and `page.rs`, may stay declarations and re-exports only.

## Bad

```text
src/
  budget_kind.rs
  budget_usage.rs
  budget/
    mod.rs
    limit.rs
```

```rust
// file: budget/mod.rs
mod limit;
pub use limit::Budget;
```

## Good

```text
src/
  budget.rs
  budget/
    kind.rs
    usage.rs
```

```rust
// file: budget.rs
mod kind;
mod usage;

pub use kind::BudgetKind;
pub use usage::BudgetUsage;

pub struct Budget { /* ... */ }
```

## Related rules

- [Leaf modules](org-leaf-module.md)
- [Non-leaf modules](org-non-leaf.md)
