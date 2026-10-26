# impl-assert-context

> Add an assertion message when it explains why the condition should hold or what failure means.

## Why it matters

An assertion can fail in logs far from the code that established its assumption. A useful message gives the reader or user the missing context.

## Bad

```rust
assert!(refund > 0);
```

## Good

```rust
assert!(
    refund > 0,
    "refund must be positive, got {refund}"
);
```

Add context when the failure may reach users, when a value helps diagnose it, or when it distinguishes caller misuse from an implementation bug. Do not repeat the condition as the message.
