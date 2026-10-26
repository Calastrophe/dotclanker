# org-leaf-module

> Name a leaf module after its main user-defined type when it has one.

## Why it matters

A matching module and filename make a type's implementation easier to find.

## Bad

```rust
// file: account_logic.rs
pub struct Account { /* ... */ }
```

## Good

```rust
// file: account.rs
pub struct Account { /* ... */ }
```

If a leaf module has no primary abstraction, name it for its contents.
