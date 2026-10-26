# name-abbreviations

> Avoid abbreviations that readers outside the project may not know.

## Why it matters

Unfamiliar abbreviations make names harder to search and understand.

## Bad

```rust
fn calc_usr_stats() { /* ... */ }
```

## Good

```rust
fn calculate_user_statistics() { /* ... */ }
```

Common, unambiguous abbreviations such as IP are acceptable. Do not abbreviate by deleting letters within a word.
