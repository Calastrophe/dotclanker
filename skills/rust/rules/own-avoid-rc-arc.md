# own-avoid-rc-arc

> Prefer borrowing and unique ownership; use shared ownership when it makes the design simpler.

## Why it matters

`Rc` and `Arc` add reference-counting work and make ownership less direct. Rust can often represent the same relationship with borrows or owned values.

## Bad

```rust
use std::rc::Rc;

struct Child {
    parent: Rc<Parent>,
}
```

## Good

```rust
struct Child<'a> {
    parent: &'a Parent,
}
```

Shared ownership is reasonable when borrowing would make the code unusually difficult to write, understand, or maintain. Cloning an `Rc` or `Arc` is not a deep copy.

## Related rules

- [Borrow instead of deep-cloning](own-borrow-deep-copy.md)
