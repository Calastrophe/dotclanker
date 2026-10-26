# name-lifetimes

> Use descriptive lifetime names when they clarify nontrivial relationships.

## Why it matters

Named lifetimes can explain how returned references relate to inputs, especially in public type definitions.

## Bad

```rust
struct Report<'a, 'b> {
    input: &'a str,
    schema: &'b str,
}
```

## Good

```rust
struct Report<'input, 'schema> {
    input: &'input str,
    schema: &'schema str,
}
```

Use an anonymous lifetime when it works. A short name such as 'a is fine for a simple input-output relationship or a local higher-ranked bound. One or two such local lifetimes are usually clear; name them when more are needed. Highly generic abstractions may also use conventional short names.
