# feature-pre-main

> Avoid implicit code execution before main.

## Why it matters

Initialization that runs before the program's ordinary entry point is harder to locate, order, test, and reason about.

## Guidance

Prefer explicit initialization from main or a clearly invoked setup function. Avoid constructors and similar mechanisms that execute during program loading unless the runtime or platform requires them.

## Bad

```rust
// With a constructor crate, this runs before main.
#[ctor::ctor]
fn initialize() {
    load_configuration();
}
```

## Good

```rust
fn main() {
    load_configuration();
    run_application();
}
```
