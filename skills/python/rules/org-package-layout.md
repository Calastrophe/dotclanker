# org-package-layout

> Keep module boundaries aligned with the project's package structure.

## Why it matters

A package should have one clear import path for each module. Ad hoc paths and duplicate module loading can create confusing type identities and hidden import-order dependencies.

## Guidance

Follow the repository's package layout and supported invocation method. Add a module where its callers expect it, and avoid importing a file by a path that bypasses its package. For a module intended to run as a package command, support `python -m package.module` or the project's configured console entry point.

## Bad

```python
import sys
sys.path.insert(0, "../src")
import helpers
```

## Good

```python
from my_app import helpers


def run() -> None:
    helpers.load_data()
```

## Caveats

Small scripts may intentionally be standalone. Do not turn them into packages unless the project needs imports, installation, or multiple entry points.
