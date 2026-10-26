# org-imports-per-module

> Group names from one module in a use statement, and keep different modules separate.

## Why it matters

Consistent import grouping makes module origins and diffs easier to scan.

## Bad

```rust
use crate::{Config, parser::Token};
```

## Good

```rust
use crate::Config;
use crate::parser::Token;
```
