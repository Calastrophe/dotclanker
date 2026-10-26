# impl-catch-unwind

> Do not use catch_unwind as ordinary error handling.

## Why it matters

It only catches unwinding panics. It cannot catch a panic when the program uses `panic=abort`, and catching a panic does not establish that affected state is safe to reuse.

## Guidance

Use `Result` for expected failures instead of `catch_unwind`. A runtime boundary may catch unwinding panics as defense in depth when the program already avoids panics, the target uses `panic=unwind`, and correctness does not depend on catching every panic or reusing state touched by a panic.

## Bad

```rust
fn parse(input: &str) -> Result<u32, String> {
    std::panic::catch_unwind(|| input.parse::<u32>())
        .map_err(|_| "parse panicked".to_owned())?
        .map_err(|error| error.to_string())
}
```

## Good

```rust
fn parse(input: &str) -> Result<u32, std::num::ParseIntError> {
    input.parse()
}
```
