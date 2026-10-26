# name-constructors

> Use conventional names for constructors and value conversions.

## Why it matters

Common names make initialization patterns easier to discover.

## Guidance

Use `new` for a standard constructor. If constructors need distinct names, use `new_with_foo`. Use `try_new` when construction can fail. For value-to-value conversion, implement `From`. Add a discoverable inherent constructor such as `from_foo` when an important conversion would otherwise be hard to find. Do not duplicate every conversion mechanically.

## Bad

```rust
struct Port(u16);

impl Port {
    fn make(value: u16) -> Self {
        Self(value)
    }
}
```

## Good

```rust
struct Port(u16);

impl From<u16> for Port {
    fn from(value: u16) -> Self {
        Self(value)
    }
}

impl Port {
    fn from_number(value: u16) -> Self {
        Self::from(value)
    }
}
```
