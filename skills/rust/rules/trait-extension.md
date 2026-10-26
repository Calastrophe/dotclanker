# trait-extension

> Keep an extension trait focused on adding methods, and coordinate it with the type it extends.

## Why it matters

Separate extension traits can introduce method-name collisions and make method resolution harder to predict.

## Guidance

Use an `Ext` suffix for a trait whose sole purpose is to add methods to an existing type. If you own the type, define the extension trait alongside it. A foundational library can own a separate extension trait for a foreign type when many callers need the same methods. Do not use `Ext` for a capability or input trait that callers use as a generic bound, even if you implement it for foreign types.

## Bad

```rust
// This trait only adds methods to std::path::Path.
trait Path {
    fn has_extension(&self, extension: &str) -> bool;
}

impl Path for std::path::Path {
    fn has_extension(&self, extension: &str) -> bool {
        self.extension() == Some(std::ffi::OsStr::new(extension))
    }
}
```

## Good

```rust
trait PathExt {
    fn has_extension(&self, extension: &str) -> bool;
}

impl PathExt for std::path::Path {
    fn has_extension(&self, extension: &str) -> bool {
        self.extension() == Some(std::ffi::OsStr::new(extension))
    }
}
```
