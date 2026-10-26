# impl-inference

> Add a type annotation when inference makes an important value hard to understand.

## Why it matters

Type inference keeps routine code concise, but generic conversions and complex iterator chains can hide the type that determines how the code behaves.

## Guidance

Annotate values when surrounding code does not make the inferred type clear, especially results of `into()`, `parse()`, or complex iterator chains. Prefer annotating the binding to adding a turbofish. If the compiler requires a turbofish, a placeholder type is often enough.

## Bad

```rust
let result = raw.parse();
```

## Good

```rust
let result: Result<usize, _> = raw.parse();
```
