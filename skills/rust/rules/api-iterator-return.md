# api-iterator-return

> Return an iterator when callers benefit from choosing how to consume the results.

## Why it matters

An iterator lets callers process values without allocating a collection or collect them into the representation they need.

## Bad

```rust
fn positive(values: &[i32]) -> Vec<&i32> {
    values.iter().filter(|&&value| value > 0).collect()
}
```

## Good

```rust
fn positive<'a>(values: &'a [i32]) -> impl Iterator<Item = &'a i32> {
    values.iter().filter(|&&value| value > 0)
}
```

Return an owned collection when most callers need one, the implementation naturally creates or returns one, callers need random access, or creating the collection is the function's purpose. A `Vec` may also be simpler when the algorithm naturally uses a loop and allocation cost is unimportant. In application code, choose `Vec` when it simplifies use and callers do not need flexibility.

## Related rules

- [Iterator inputs](api-iterator-params.md)
