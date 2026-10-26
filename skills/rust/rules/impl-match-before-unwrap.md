# impl-match-before-unwrap

> Use control flow that proves an Option or Result case before accessing its value.

## Why it matters

Checking a value and then unwrapping it can leave a panic path in code whose surrounding logic already establishes the case.

## Bad

```rust
if option.is_some() {
    use_value(option.unwrap());
}
```

## Good

```rust
if let Some(value) = option {
    use_value(value);
}
```

Avoid a redundant panic path when the input is already known to match. This does not ban `unwrap`; follow project policy and use a deliberate invariant check when that is clearer.

## Related rules

- [Explain required panics](impl-expect.md)
