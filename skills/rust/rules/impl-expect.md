# impl-expect

> If a panic is necessary for an invariant, use expect to explain the reason.

## Why it matters

Unlike unwrap, expect can state the invariant that should make the operation succeed. That message helps diagnose a violated assumption.

## Bad

```rust
let config = CONFIG.get().unwrap();
```

## Good

```rust
let config = CONFIG.get().expect("configuration is initialized before request handling");
```

Do not use expect for an ordinary recoverable error. Return or handle that error instead.

## Related rules

- [Choose an error-reporting mechanism](api-error-reporting.md)
