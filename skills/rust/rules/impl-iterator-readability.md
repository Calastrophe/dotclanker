# impl-iterator-readability

> Use a direct loop when it makes complex iterator processing easier to follow.

## Why it matters

Long chains with branching, mutation, or error accounting can obscure the order of work.

## Bad

```rust
items.iter()
    .filter_map(parse)
    .filter(is_ready)
    .for_each(|item| {
        if should_process(&item) {
            process(item);
            processed_count += 1;
            log_processed();
        }
    });
```

## Good

```rust
for item in items.iter() {
    if let Some(item) = parse(item) {
        if is_ready(&item) && should_process(&item) {
            process(item);
            processed_count += 1;
            log_processed();
        }
    }
}
```

Use a `for` loop instead of `for_each` for ordinary side effects when the closure adds branching or multiple actions. Keep short chains and focused operations such as `any()` when they express the logic better than a loop.
