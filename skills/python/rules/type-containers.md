# type-containers

> Choose collections and record types that show the operations and fields the code needs.

## Why it matters

A list, tuple, set, and dictionary express different behavior. The right collection makes uniqueness, lookup, ordering, and mutability visible. Named record fields help readers understand each value without remembering its position.

## Guidance

Use a list for an ordered sequence that may change, a tuple for an immutable sequence, a set for uniqueness or membership, and a dictionary for key-to-value lookup. Avoid encoding a record as positional values when names would make it easier to understand.

Use a data class for plain records. Set `frozen=True` when preventing attribute reassignment is part of the contract. Use `field(default_factory=...)` for mutable per-instance defaults. Choose a regular class when construction validates complex invariants, manages resources, or coordinates changing state.

While iterating over a dictionary or set, do not add or remove keys or elements. Replacing the value for an existing dictionary key does not change its keys, though later reads in the loop will see the new value. See [fn-iterators](fn-iterators.md) for snapshots when a loop must change its source.

## Bad

### Positional record

```python
def report(user: tuple[str, int, bool]) -> str:
    return f"{user[0]}: {user[1]} ({user[2]})"
```

### Rejected mutable field default

```python
from dataclasses import dataclass


@dataclass
class Job:
    # Dataclasses reject this mutable default at class definition.
    tags: list[str] = []
```

## Good

### Named record

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class User:
    name: str
    age: int
    active: bool


def report(user: User) -> str:
    return f"{user.name}: {user.age} ({user.active})"
```

### Per-instance mutable field

```python
from dataclasses import dataclass, field


@dataclass
class Job:
    tags: list[str] = field(default_factory=list)
```

## Caveats

Tuples can contain mutable values, and a frozen data class prevents rebinding its fields but does not recursively freeze mutable field values. Use a specialized collection when its semantics match the domain better than a built-in type. Do not add generated comparisons or ordering unless those semantics make sense for the record.
