# name-generic-params

> Keep generic parameter names short and related to what they represent.

## Why it matters

Short names keep familiar abstractions readable. A meaningful name helps when a parameter has a specific role or strong trait bounds.

## Bad

```rust
struct Map<KeyType, ValueType> {
    entries: Vec<(KeyType, ValueType)>,
}
```

## Good

```rust
struct Map<K, V> {
    entries: Vec<(K, V)>,
}

struct Parser<Input, Output> {
    parse: fn(Input) -> Output,
}
```

Use T or U when the parameter is generic in a broad sense. When bounds identify a role, a short related name can help, such as E for an error or I for an iterator.
