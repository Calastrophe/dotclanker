# type-named-fields

> Prefer named fields when tuple positions would make a value hard to read.

## Why it matters

Names explain each field's role without making readers remember what position zero or one means.

## Bad

```rust
struct Window(String, u32, u32);
```

## Good

```rust
struct Window {
    title: String,
    width: u32,
    height: u32,
}
```

One-field newtypes and tuples used only briefly in a local expression are natural exceptions.
