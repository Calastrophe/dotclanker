# feature-doc-hidden

> Treat doc-hidden items in code you do not own as private implementation details.

## Why it matters

An item hidden from generated documentation is not presented as part of the supported API. Depending on it can tie code to implementation details that may change.

## Guidance

Do not access another crate's `#[doc(hidden)]` items. If you own the code and need one internally, follow the project's intended use and compatibility policy.

## Bad

```rust
// This dependency item is marked #[doc(hidden)].
let decoded = dependency::hidden_decode(input);
```

## Good

```rust
// Use the dependency's documented API.
let decoded = dependency::decode(input);
```
