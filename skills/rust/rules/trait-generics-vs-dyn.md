# trait-generics-vs-dyn

> In reusable library inputs, prefer generics unless dynamic dispatch or smaller code is a better fit.

## Why it matters

Generics enable static dispatch and can allow inlining. A `dyn` trait uses runtime dispatch, which can reduce code duplication when one implementation serves many concrete types.

## Bad

```rust
// A reusable library input with no need for runtime selection.
fn write_report(out: &mut dyn std::fmt::Write) {
    // ...
}
```

## Good

```rust
fn write_report<W: std::fmt::Write>(out: &mut W) {
    // ...
}
```

Use `dyn` when runtime selection, smaller code, or trait-object storage matters. Check that the trait is dyn-compatible. In applications, choose based on actual call and storage needs.

## Related rules

- [Choose traits or inherent methods](trait-or-inherent.md)
