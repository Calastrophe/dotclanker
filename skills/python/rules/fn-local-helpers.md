# fn-local-helpers

> Split a function when a named step clarifies its work or can be understood and changed on its own.

## Why it matters

A focused function makes its inputs, outputs, and side effects easier to reason about. Excessive splitting can hide a simple flow behind many tiny calls.

## Guidance

Keep related steps together when their sequence is the clearest explanation. Extract a helper when it names a meaningful operation, removes real duplication, or isolates a useful unit. Put reusable behavior at module or class scope. Nest a helper when it is private to one operation and its closure over local state improves clarity.

## Bad

```python
from collections.abc import Iterable, Mapping
from pathlib import Path


def export_contacts(rows: Iterable[Mapping[str, str]], destination: Path) -> int:
    lines = []
    for row_number, row in enumerate(rows, start=1):
        name = row.get("name", "").strip()
        email = row.get("email", "").strip().casefold()
        if not name:
            raise ValueError(f"row {row_number}: missing name")
        if "@" not in email:
            raise ValueError(f"row {row_number}: invalid email")
        lines.append(f"{name}\t{email}")
    destination.write_text("\n".join(lines), encoding="utf-8")
    return len(lines)
```

## Good

```python
from collections.abc import Iterable, Mapping
from pathlib import Path


def format_contact_row(row_number: int, row: Mapping[str, str]) -> str:
    name = row.get("name", "").strip()
    email = row.get("email", "").strip().casefold()
    if not name:
        raise ValueError(f"row {row_number}: missing name")
    if "@" not in email:
        raise ValueError(f"row {row_number}: invalid email")
    return f"{name}\t{email}"


def export_contacts(rows: Iterable[Mapping[str, str]], destination: Path) -> int:
    lines = [
        format_contact_row(row_number, row)
        for row_number, row in enumerate(rows, start=1)
    ]
    destination.write_text("\n".join(lines), encoding="utf-8")
    return len(lines)
```

The helper names one operation, formatting and validating a contact row. `export_contacts` still controls iteration, file output, and the returned count.

## Caveats

There is no universal line-count limit. Keep long functions when extracting pieces would obscure the sequence or split tightly coupled state; shorten them when unrelated decisions are mixed together.
