# doc-public

> Document public APIs, panic cases, and unsafe invariants.

## Why it matters

Rustdoc and editor tooling use documentation comments to explain behavior and requirements to callers.

## Guidance

Use `///` for public items and `//!` for crate or module documentation. Use Markdown with ordinary capitalization and punctuation. Include examples that explain crate behavior, but do not force one onto every method. Document when malformed input can panic and the invariants unsafe functions and traits require. Avoid explaining Rust basics or repeating clear names. Self-explanatory enum variants can go undocumented.

Use ordinary `//` comments for implementation notes. Prefer source comments over generated documentation when generation offers no clear benefit.

## Bad

```rust
/// Removes an element.
pub fn remove_element() -> Element {
    // ...
}
```

## Good

```rust
/// Removes and returns the element at index.
///
/// # Panics
/// Panics if index is outside the collection.
pub fn remove(index: usize) -> Element {
    // ...
}
```
