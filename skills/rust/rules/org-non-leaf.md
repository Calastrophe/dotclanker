# org-non-leaf

> Keep non-leaf modules that have no main type of their own focused on declarations and re-exports.

## Why it matters

Separating module structure from implementation helps readers find each abstraction's definition.

## Guidance

Use a non-leaf module with no main type of its own to declare child modules and re-export their public items. A module that has a main type defines it in the module file; see [org-module-directory](org-module-directory.md). A small amount of glue connecting child modules is reasonable. Keep other substantial implementations in leaf modules. The project's layout may treat test and language-interop modules as special leaf modules.

## Bad

```rust
mod parser {
    pub mod token;

    pub fn parse(input: &str) {
        // A large implementation that is not the module's main type lives alongside module wiring.
    }
}
```

## Good

```rust
mod parser;
pub use parser::parse;
```
