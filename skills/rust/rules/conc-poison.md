# conc-poison

> Do not reach inside a poisoned mutex to use its protected state.

## Why it matters

Poisoning means a thread panicked while holding the lock. The protected data may violate invariants other code relies on.

## Bad

```rust
let guard = mutex.lock().unwrap_or_else(|poisoned| poisoned.into_inner());
use_state(&guard);
```

## Good

```rust
let guard = mutex.lock()?;
use_state(&guard);
```

Propagate the poison error or abandon the operation. Do not use the poisoned value as though the lock were healthy.
