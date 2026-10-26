# class-properties

> Use properties for cheap, predictable attribute access, and methods for visible work.

## Why it matters

Callers expect reading an attribute to be simple. Hiding I/O, mutation, or expensive computation behind a property makes that cost hard to see.

## Guidance

Expose ordinary stored values as attributes. Use a property for a derived value or validation that remains cheap and unsurprising. Use a method when callers should see that an operation takes time, performs I/O, or changes state.

## Bad

```python
class Account:
    @property
    def balance(self) -> int:
        return self.load_balance_from_database()
```

## Good

```python
class Account:
    def load_balance(self) -> int:
        return self.database.read_balance(self.account_id)
```

## Caveats

A property can preserve a stable attribute interface while its implementation changes. If a property setter is needed, keep assignment behavior unsurprising and document any meaningful validation.
