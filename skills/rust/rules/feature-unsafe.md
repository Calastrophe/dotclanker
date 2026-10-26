# feature-unsafe

> Use unsafe only when safe Rust cannot express the required behavior.

## Why it matters

Unsafe code makes the programmer responsible for upholding invariants the compiler cannot check.

## Guidance

Encapsulate unsafe operations in the smallest safe abstraction that can enforce their contract. Keep unsafe code separate from ordinary business logic when practical, ideally in a small module or crate with few dependencies. Add a `// SAFETY:` comment to each unsafe block and document requirements on unsafe functions and traits. Explain why safe Rust cannot do the job. If performance is the reason, support it with benchmarks.

## Bad

```rust
unsafe {
    // No explanation of the required invariants.
    read_pointer(ptr)
}
```

## Good

```rust
/// Reads a value from `ptr`.
///
/// # Safety
///
/// `ptr` must be aligned, non-null, and point to an initialized `T` in memory
/// that remains readable for this call. No concurrent write may race with the read.
unsafe fn read_value<T: Copy>(ptr: *const T) -> T {
    // SAFETY: This function's caller must uphold the documented pointer contract.
    unsafe { ptr.read() }
}
```

The `Copy` bound prevents this read from duplicating ownership of a value such as a `String`.

## Related rules

- [Document unsafe invariants](doc-public.md)
