# type-newtype

> Use a newtype instead of an alias when the type needs distinct meaning, validation, or its own API.

## Why it matters

An alias does not create a new type, so the compiler cannot prevent two semantically different values from being mixed.

## Bad

```rust
type UserId = u64;
type OrderId = u64;

fn cancel(order: UserId) { /* ... */ }
```

## Good

```rust
struct UserId(u64);
struct OrderId(u64);

fn cancel(order: OrderId) { /* ... */ }
```

An alias can shorten a long type name when no stronger type boundary is needed.

## Related rules

- [Common trait implementations](trait-common-impls.md)
