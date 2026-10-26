# feature-toolchain

> Follow the repository's stable or nightly toolchain policy.

## Why it matters

Unstable features need a nightly compiler and may change before they reach stable Rust. Choose that tradeoff according to the project's release and support needs.

## Guidance

Do not introduce unstable features into a project that targets stable Rust. If the project explicitly uses nightly, follow its feature allowlist and review process. Keep its toolchain policy consistent with build and deployment targets.

## Bad

```toml
# In a repository that supports stable Rust.
[toolchain]
channel = "nightly"
```

## Good

```toml
# In a repository that supports stable Rust.
[toolchain]
channel = "stable"
```
