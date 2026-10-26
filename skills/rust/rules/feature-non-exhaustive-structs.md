# feature-non-exhaustive-structs

> Mark a public struct with public fields non_exhaustive when adding fields must remain compatible.

## Why it matters

Callers construct a struct with public fields using a literal. Adding another field can break those callers.

## Guidance

For an evolving library API, consider `#[non_exhaustive]` on a public struct with public fields. External callers cannot use struct literals, including functional record update syntax. They can use a constructor or `Default::default()` and then set public fields, or use a builder. If downstream callers need struct update syntax, leave the struct exhaustive.

Do not add it mechanically. A fixed ontology, a deliberate promise of struct-literal construction, or a planned migration that updates all callers can make an exhaustive struct the better choice.

## Bad

```rust
// A published library expects this public struct to gain fields.
pub struct Config {
    pub retries: u32,
}

let config = Config { retries: 3 };
```

## Good

```rust
// External callers can set fields but cannot use struct literals.
#[non_exhaustive]
#[derive(Default)]
pub struct Config {
    pub retries: u32,
}

let mut config = Config::default();
config.retries = 3;
```
