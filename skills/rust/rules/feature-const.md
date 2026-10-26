# feature-const

> Mark a public function const only when you expect that promise to last.

## Why it matters

Changing a public const function into a regular function can break callers that use it in a constant expression.

## Guidance

Before adding `const` to a public function, consider whether it is likely to remain valid for compile-time evaluation. Treat `const` as part of the API contract, not a harmless optimization.

## Bad

```rust
// Add const only because this body currently permits it.
pub const fn retry_limit() -> usize {
    3
}
```

## Good

```rust
// Keep the API non-const when callers do not need compile-time evaluation.
pub fn retry_limit() -> usize {
    3
}
```
