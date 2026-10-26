# trait-derive

> Use a derive when it expresses the implementation you intend.

## Why it matters

Derives keep routine trait implementations short and consistent with the trait's standard behavior.

## Bad

```rust
struct Point(i32, i32);

impl Copy for Point {}
impl Clone for Point {
    fn clone(&self) -> Self {
        Self(self.0, self.1)
    }
}
```

## Good

```rust
#[derive(Clone, Copy)]
struct Point(i32, i32);
```

Implement the trait manually when derived behavior would be incorrect or misleading.

## Related rules

- [Implement standard traits](trait-standard.md)
