# feature-must-use-functions

> Mark a function must_use when ignoring its return value can make the caller incorrect.

## Why it matters

Some pure functions have no useful effect unless their returned value is used. A `#[must_use]` annotation helps catch callers that discard a result that carries the function's purpose.

## Bad

```rust
fn normalized_name(input: &str) -> String {
    input.trim().to_lowercase()
}

normalized_name(" Alice "); // The normalized value is discarded.
```

## Good

```rust
#[must_use]
fn normalized_name(input: &str) -> String {
    input.trim().to_lowercase()
}

fn main() {
    normalized_name(" Alice "); // Warning: the normalized value is discarded.
    let name = normalized_name(" Alice ");
    println!("{name}");
}
```

Use the attribute for pure functions whose result matters, or when the result carries an error or important information. Side effects alone do not justify the annotation. A removal operation, for example, may allow callers to discard the removed value. Do not repeat the annotation when the return type is already `#[must_use]`, as `Result` is.

## Related rules

- [Mark must-use types](feature-must-use-types.md)
