# api-into-params

> Consider impl Into<T> for a library input when call-site ergonomics matter more than a simpler signature.

## Why it matters

An `Into` parameter accepts compatible input types without requiring each caller to write a conversion.

## Bad

```rust
fn set_label(label: String) {
    // Store the label.
}

set_label(String::from("primary"));
```

## Good

```rust
fn set_label(label: impl Into<String>) {
    let label = label.into();
    // Store the owned label.
}

set_label("primary");
```

Use `T` directly when callers already have that type, or when generic machinery, code size, or type inference would make the API harder to use. Application code rarely needs this flexibility.

## Related rules

- [API scope](../SKILL.md#scope-and-conventions)
