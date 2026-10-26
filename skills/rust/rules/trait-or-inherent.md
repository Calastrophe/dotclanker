# trait-or-inherent

> Use inherent methods for type-specific behavior. Define traits for polymorphism or extension.

## Why it matters

An unnecessary trait adds an abstraction and implementation boundary without providing generic use or substitutability.

## Guidance

Use a trait when callers need generic bounds, trait-based input or output, or an extension trait. Otherwise, put behavior in an inherent impl on the type.

## Bad

```rust
trait CacheOperations {
    fn clear(&mut self);
}

impl CacheOperations for Cache {
    fn clear(&mut self) { /* ... */ }
}
```

## Good

```rust
impl Cache {
    fn clear(&mut self) { /* ... */ }
}
```
