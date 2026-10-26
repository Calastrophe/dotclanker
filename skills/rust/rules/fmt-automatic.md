# fmt-automatic

> Use the formatter and settings selected by the repository.

## Why it matters

Automated formatting keeps style consistent and saves review time on layout.

## Guidance

Run the formatter documented by the project. For standard Rust formatting, follow the repository's `rustfmt` configuration and stable MSRV. If the repository has no conflicting choices, use a 100-column limit, lowercase hexadecimal digits, format macro matchers when the supported `rustfmt` version offers that option, and escape invisible characters in literals. Keep settings portable. Do not add unstable options to a stable project configuration.

## Bad

```rust
fn total(values:&[String])->usize{values.iter().map(|value|value.len()).sum()}
```

## Good

```rust
fn total(values: &[String]) -> usize {
    values.iter().map(|value| value.len()).sum()
}
```
