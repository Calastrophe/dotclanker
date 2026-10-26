# api-iterator-params

> Let reusable library functions accept iterators when they only need to traverse inputs.

## Why it matters

Callers can pass values from different collections or iterator chains without copying them into a slice first.

## Bad

```rust
fn total(values: &[u64]) -> u64 {
    values.iter().sum()
}
```

## Good

```rust
fn total(values: impl IntoIterator<Item = u64>) -> u64 {
    values.into_iter().sum()
}
```

An application function can take a slice when its callers are known and that gives a simpler interface. Add iterator flexibility only when it has a use.

## Related rules

- [Iterator returns](api-iterator-return.md)
