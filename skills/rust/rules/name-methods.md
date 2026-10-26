# name-methods

> Follow standard Rust naming conventions for methods and conversions.

## Why it matters

Conventional names make method behavior easier to infer and help callers distinguish borrowing, owned conversion, and ownership-consuming conversion.

## Guidance

Follow Rust standard library naming patterns. Use `as_` for cheap borrowed views, `to_` for owned or computed conversions, and `into_` when the method consumes `self`.

## Bad

```rust
struct Label(String);

impl Label {
    fn as_str(&self) -> String {
        self.0.clone()
    }

    fn to_lowercase(&self) -> &str {
        &self.0
    }

    fn into_string(&self) -> String {
        self.0.clone()
    }
}
```

## Good

```rust
struct Label(String);

impl Label {
    fn as_str(&self) -> &str {
        &self.0
    }

    fn to_lowercase(&self) -> String {
        self.0.to_lowercase()
    }

    fn into_string(self) -> String {
        self.0
    }
}
```
