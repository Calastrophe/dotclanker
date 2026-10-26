# org-local-trait-import

> Import a trait inside a function when its methods are used only there.

## Why it matters

A local import shows why a method is available without adding a file-wide name used only there.

## Bad

```rust
use std::str::FromStr;

fn parse_byte(text: &str) -> Result<u8, std::num::ParseIntError> {
    u8::from_str(text)
}
```

## Good

```rust
fn parse_byte(text: &str) -> Result<u8, std::num::ParseIntError> {
    use std::str::FromStr;
    u8::from_str(text)
}
```

Keep the import at module scope when many functions use the trait.
