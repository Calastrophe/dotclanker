# trait-common-impls

> Implement common traits when their contracts fit the type and its likely use.

## Why it matters

Standard traits support ordinary operations and collections, but a trait should never promise semantics the type cannot keep.

## Guidance

- Consider traits implemented by all fields for public-field types.
- Make small error types Copy when their fields allow it.
- Implement Copy when the type is small and it is reasonable to expect it to remain Copy. Most types should implement Clone unless they represent an external resource.
- Implement Default when a type has a sensible default. Public options structs benefit when callers can override a few fields with `..Default::default()`.
- Implement PartialEq when equality is useful. Implement Eq only when equality is reflexive. Implement PartialOrd for partial ordering and Ord only for a true total ordering.
- Types likely to be map keys should implement Eq and Hash. Add Ord only when the type has a clear ordering.

Do not invent defaults when their meaning is unclear or unsafe.

## Bad

```rust
pub struct SearchOptions {
    pub case_sensitive: bool,
    pub limit: Option<usize>,
}

let options = SearchOptions {
    case_sensitive: true,
    limit: None,
};
```

## Good

```rust
#[derive(Default)]
pub struct SearchOptions {
    pub case_sensitive: bool,
    pub limit: Option<usize>,
}

let options = SearchOptions {
    case_sensitive: true,
    ..Default::default()
};
```
