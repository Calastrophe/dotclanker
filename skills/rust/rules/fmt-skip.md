# fmt-skip

> Skip automatic formatting only when formatting would harm the generated output or its readability.

## Why it matters

Personal formatting exceptions make a codebase less consistent and reduce the formatter's value.

## Bad

```rust
#[rustfmt::skip]
fn add(left: i32, right: i32) -> i32 { left + right }
```

## Good

```rust
fn add(left: i32, right: i32) -> i32 {
    left + right
}
```

Use a skip attribute only when fixed visual alignment materially helps readers or the code is generated. Format ordinary source.
