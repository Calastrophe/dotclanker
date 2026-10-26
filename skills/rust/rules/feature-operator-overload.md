# feature-operator-overload

> Overload operators for their expected mathematical or logical meaning.

## Why it matters

Readers infer familiar semantics from operators. Using one only as shorter syntax can make expressions surprising.

## Bad

```rust
use std::ops::Add;

struct Logger;

impl Add for Logger {
    type Output = ();

    fn add(self, _rhs: Self) {
        // Prints a log message rather than adding values.
    }
}
```

## Good

Use a named method for behavior that is not addition, comparison, or another expected operator meaning.
