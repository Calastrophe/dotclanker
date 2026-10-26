# org-non-leaf

> Keep non-leaf modules focused on declarations and re-exports.

## Why it matters

Separating module structure from implementation helps readers find each abstraction's definition.

## Guidance

Use a non-leaf module to declare child modules and re-export their public items. A small amount of glue connecting child modules is reasonable. Keep substantial implementations in leaf modules. The project's layout may treat test and language-interop modules as special leaf modules.

## Bad

```rust
mod parser {
    pub mod token;

    pub fn parse(input: &str) {
        // A large parser implementation lives alongside module wiring.
    }
}
```

## Good

```rust
mod parser;
pub use parser::parse;
```
