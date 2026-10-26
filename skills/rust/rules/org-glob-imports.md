# org-glob-imports

> Prefer explicit imports so readers can identify where names come from.

## Why it matters

Glob imports can hide where names come from and make large mechanical edits harder to review.

## Guidance

Avoid glob imports by default. Narrow exceptions make sense when intent is clear:

- Use super::* in a small inline test module.
- Use enum::* inside a method or function primarily concerned with that enum.
- Re-export a submodule's public API from a facade module.
- Follow a third-party crate's prelude convention when its use is established.
- In C++ interop, merge namespaces only when needed to make a Rust module reflect the C++ namespace. Keep the merged module private.

Avoid defining a prelude solely to spread names through a crate.

## Bad

```rust
use std::collections::*;
```

## Good

```rust
use std::collections::{HashMap, HashSet};
```
