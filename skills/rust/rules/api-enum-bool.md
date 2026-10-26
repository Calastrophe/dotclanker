# api-enum-bool

> Use a named enum when a boolean argument does not explain its meaning at the call site.

## Why it matters

At a call site, `true` does not say whether a function enables caching, follows links, or removes a file.

## Bad

```rust
render_page(page, true);
```

## Good

```rust
enum LinkMode {
    Follow,
    Ignore,
}

render_page(page, LinkMode::Follow);
```

Use a boolean when its purpose is clear from the parameter name or nearby syntax. A short comment can clarify it when an enum adds no useful meaning.
