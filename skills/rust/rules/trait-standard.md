# trait-standard

> Implement a standard library trait when its contract matches the behavior.

## Why it matters

Standard traits let callers use familiar generic operations instead of learning a custom method that does the same thing.

## Bad

```rust
impl Token {
    fn from_str(text: &str) -> Result<Self, ParseTokenError> {
        // ...
    }
}
```

## Good

```rust
impl std::str::FromStr for Token {
    type Err = ParseTokenError;

    fn from_str(text: &str) -> Result<Self, Self::Err> {
        // ...
    }
}
```

An inherent method can coexist with the trait implementation if it improves discoverability or offers a distinct contract.

## Related rules

- [Use derives when they fit](trait-derive.md)
