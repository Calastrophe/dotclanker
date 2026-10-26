# feature-accessors

> Name getters, setters, and scoped accessors consistently.

## Guidance

- Use `foo()` for a getter returning `&T` or a `Copy` value.
- Use `foo_mut()` for a mutable borrow.
- Use `take_foo()` when the method returns the old value and leaves a default or empty state.
- Use `set_foo(value)` for a setter and `replace_foo(value)` when it also returns the previous value.
- Use `with_foo` or `with_mut_foo` for an accessor that accepts a closure. If you need only one closure accessor, `with_foo` can also accept a mutable borrow.

Closure accessors can limit a `RefCell` or `Mutex` borrow without exposing a guard type. They can validate or process a mutation when a plain setter cannot, and `with_foo` can return the closure's result.

## Bad

```rust
impl User {
    fn get_name(&self) -> &str {
        &self.name
    }

    fn get_name_mut(&mut self) -> &mut String {
        &mut self.name
    }
}
```

## Good

```rust
impl User {
    fn name(&self) -> &str {
        &self.name
    }

    fn name_mut(&mut self) -> &mut String {
        &mut self.name
    }
}
```
