# org-qualify-functions

> Qualify a free-function call with its module when that makes the call's origin clearer.

## Why it matters

The module path can distinguish a dependency call from a local function without IDE cross-references.

## Bad

```rust
use std::mem::take;

let old = take(&mut value);
```

## Good

```rust
use std::mem;

let old = mem::take(&mut value);
```

Use the unqualified name when its origin is clear, the module name collides, or the function conventionally reads better on its own.
