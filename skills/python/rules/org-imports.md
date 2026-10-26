# org-imports

> Make an imported name's source clear and keep imports consistent with the package.

## Why it matters

Clear imports help readers tell local modules from third-party code and make package boundaries easier to maintain.

## Guidance

Prefer importing a module when that keeps the source visible at its use site. Import individual names when they are few and unambiguous. Use aliases to resolve a real collision or to follow a well-known library convention. Follow the project's choice of absolute or relative imports; do not change it just for a local edit.

Keep imports at module scope unless a local import avoids a real optional-dependency or cycle problem. Treat a cycle as a design problem first. Do not use `sys.path` edits to make an import work.

## Bad

```python
from helpers import *


def render(value):
    return format_value(value)
```

## Good

```python
from app import formatting


def render(value):
    return formatting.format_value(value)
```

## Caveats

A focused local import can be reasonable when a dependency is optional or expensive to import. Keep the reason apparent and avoid scattering conditional imports through ordinary code.
