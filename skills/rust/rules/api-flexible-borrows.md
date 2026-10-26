# api-flexible-borrows

> Accept a reference to the data instead of a reference to its usual container.

## Why it matters

Flexible reference types let callers pass data from different containers without copying it.

## Bad

```rust
fn first_name(names: &Vec<String>) -> Option<&String> {
    names.first()
}
```

## Good

```rust
fn first_name(names: &[String]) -> Option<&str> {
    names.first().map(String::as_str)
}
```

Prefer `&T` over `&Box<T>`, `&[T]` over `&Vec<T>`, `&str` over `&String`, and `Option<&T>` over `&Option<T>`. Choose another input when the data structure itself matters, such as for non-linear text.

## Related rules

- [Borrow instead of deep-cloning](own-borrow-deep-copy.md)
