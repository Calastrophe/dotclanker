# feature-api-pairs

> Provide the companion methods that callers reasonably expect.

## Why it matters

An API feels incomplete when it provides a common operation but omits its standard counterpart.

## Guidance

If a collection provides `len()`, also provide `is_empty()`. Follow this pattern for other API pairs when the language ecosystem has a clear expectation.

## Bad

```rust
impl Queue {
    fn len(&self) -> usize {
        self.items.len()
    }
}
```

## Good

```rust
impl Queue {
    fn len(&self) -> usize {
        self.items.len()
    }

    fn is_empty(&self) -> bool {
        self.items.is_empty()
    }
}
```
