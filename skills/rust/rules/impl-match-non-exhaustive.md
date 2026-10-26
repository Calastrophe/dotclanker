# impl-match-non-exhaustive

> Make handling of known and future enum variants clear.

## Why it matters

An explicit match documents which current cases matter. A wildcard arm still handles variants added later by an upstream crate.

## Bad

```rust
// `Shape` is a non_exhaustive enum from another crate.
match shape {
    Shape::Circle => draw_circle(),
    Shape::Square => draw_square(),
}
```

## Good

```rust
match shape {
    Shape::Circle => draw_circle(),
    Shape::Square => draw_square(),
    Shape::Triangle => {},
    _ => {}
}
```

When only one variant matters, `if let` or `let-else` may be clearer. Use `match` when selected variants read better that way. A wildcard arm can ignore variants that need no action; do not omit it when matching a `#[non_exhaustive]` enum from another crate.

## Related rules

- [Mark public enums that may evolve](feature-non-exhaustive-enums.md)
