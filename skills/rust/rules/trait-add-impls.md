# trait-add-impls

> Add trait implementations when they meet expected use, with more foresight in foundational libraries.

## Why it matters

Application types can grow trait support in response to real use. Low-level libraries often need to anticipate what downstream users will reasonably need.

## Guidance

In applications, add trait implementations as needed. In low-level or widely reused libraries, anticipate common user needs when designing the type, but avoid speculative implementations that do not fit its semantics.

## Bad

```rust
// Callers need to compare and hash this public identifier, but it has no
// corresponding trait implementations.
pub struct RequestId(u64);
```

## Good

```rust
#[derive(PartialEq, Eq, Hash)]
pub struct RequestId(u64);
```
