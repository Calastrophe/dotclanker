# api-error-reporting

> Choose Option, Result, or panic according to what the caller can know and do.

## Why it matters

An API's failure type tells callers whether a result may be absent, why an operation failed, or whether code broke an invariant.

## Guidance

Use `Option` when an outcome may be absent and needs no failure detail. Use `Result` when callers can recover and need the reason, such as an I/O error. Use `panic` for a violated invariant or programming bug that should be fixed. A project may require `Result` even for invariant failures. Developer-facing binaries may panic on nonfatal errors, and tests may panic for assertions.

## Bad

```rust
fn open(path: &Path) -> Option<std::fs::File> {
    std::fs::File::open(path).ok()
}
```

## Good

```rust
fn open(path: &Path) -> Result<std::fs::File, std::io::Error> {
    std::fs::File::open(path)
}
```

## Related rules

- [Explain required panics](impl-expect.md)
- [Avoid check-then-unwrap](impl-match-before-unwrap.md)
