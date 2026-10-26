# org-import-types

> Import types and traits for unqualified use by default.

## Why it matters

Short type names keep signatures readable. Qualification can resolve ambiguity.

## Bad

```rust
fn load() -> Result<(), std::io::Error> {
    Ok(())
}
```

## Good

```rust
use std::io::Error;

fn load() -> Result<(), Error> {
    Ok(())
}
```

Use a path or alias when a common name is ambiguous. Qualification can also help with a large family of related types, in generated code and macros, or in files with many dependencies where importing every type would add more clutter than clarity.
