# macro-prefer-functions

> Prefer ordinary Rust constructs, then declarative macros, before heavier code generation.

## Why it matters

Macros can make code harder for IDEs, search tools, reviewers, and large-scale refactoring tools to understand. They are also harder to write correctly.

## Guidance

Use a function, type, trait, object, or module when it can express the API. Use macro_rules when simple syntax-driven generation is needed. Choose another generator or a procedural macro only when its extra power justifies the cost.

## Bad

```rust
macro_rules! increment {
    ($value:expr) => {
        $value + 1
    };
}

let next = increment!(value);
```

## Good

```rust
fn increment(value: i32) -> i32 {
    value + 1
}

let next = increment(value);
```
