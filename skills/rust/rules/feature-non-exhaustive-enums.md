# feature-non-exhaustive-enums

> Mark a public enum non_exhaustive when downstream callers must tolerate future variants.

## Why it matters

Adding an enum variant can break downstream exhaustive matches. non_exhaustive requires external callers to account for variants they do not know yet.

## Guidance

For a published library enum that may evolve, use #[non_exhaustive]. It is unnecessary for a fixed ontology or a deliberately closed set, such as a two-state type that could be a boolean. A project may instead choose to add variants through a planned migration when its compatibility policy allows it. This matters most when many external callers may match the enum. A small number of known callers may make the migration cost acceptable.

## Bad

```rust
// A published library enum that may gain variants.
pub enum TransportError {
    Disconnected,
    Timeout,
}
```

## Good

```rust
#[non_exhaustive]
pub enum TransportError {
    Disconnected,
    Timeout,
}
```

## Related rules

- [Match non-exhaustive enums](impl-match-non-exhaustive.md)
