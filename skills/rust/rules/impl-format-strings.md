# impl-format-strings

> Use standard format strings for multi-part string construction and literal substitution.

## Why it matters

Format strings express substitutions directly and avoid inventing a small template language. Writing into an existing buffer can also avoid an extra allocation.

## Bad

```rust
let greeting = "Hello, ".to_owned() + name + "!";
let line = format!("item: {item}");
output.push_str(&line);
output.push('\n');
```

## Good

```rust
let greeting = format!("Hello, {name}!");
use std::fmt::Write;
writeln!(&mut output, "item: {item}")?;
```

Prefer `format!` to repeated `+` operations when combining more than two strings. Use `write!` or `writeln!` with an existing formatter or buffer. Use format strings for literal templates with substitutions instead of `str::replace`.
