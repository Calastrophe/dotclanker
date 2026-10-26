# own-borrow-deep-copy

> Borrow data instead of deep-cloning it when the code only needs to read or pass it along.

## Why it matters

A deep clone may allocate and copy all of an object's data. Borrowing avoids that cost when the code does not need ownership.

## Bad

```rust
fn count_words(text: &String) -> usize {
    let copy = text.clone();
    copy.split_whitespace().count()
}
```

## Good

```rust
fn count_words(text: &str) -> usize {
    text.split_whitespace().count()
}
```

Clone when the caller needs an owned value, borrowing becomes unreasonably complex, or measurements and lifetime constraints justify it. Cloning an `Rc` or `Arc` only increments the reference count.

## Related rules

- [Accept flexible references](api-flexible-borrows.md)
