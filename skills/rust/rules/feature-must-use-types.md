# feature-must-use-types

> Mark a type must_use when callers need to consume a returned value for the operation to matter.

## Why it matters

Discarding an error or a lazy computation can silently skip the caller's intended work.

## Bad

```rust
struct PendingWrite {
    // The write runs only when this value is committed.
}
```

## Good

```rust
#[must_use]
struct PendingWrite {
    // The write runs only when this value is committed.
}
```

Use the attribute for error types and values that represent deferred work. Do not apply it to every type without a clear cost to ignoring the value.

## Related rules

- [Mark must-use functions](feature-must-use-functions.md)
