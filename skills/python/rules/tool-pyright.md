# tool-pyright

> Use Pyright as a static checker for the Python code in scope, with its interpreter, imports, and target version matched to the project.

## Default

Prefer Pyright for new projects and projects without an established static checker. Preserve an existing checker unless changing it is part of the task.

Run the project-managed checker when available, for example `uv run --locked pyright`.

If Pyright is not already declared and adding a persistent development dependency is in scope, `uv add --dev pyright` installs the third-party `pyright` Python wrapper from PyPI. That wrapper invokes the upstream Pyright checker. Microsoft does not publish the wrapper. The upstream CLI is also distributed through npm. Use the distribution already chosen by the project rather than mixing installation methods.

## Why it matters

Pyright can catch inconsistent types and invalid control flow without running the program. It can resolve imports and interpret syntax correctly only when its environment and configuration match the project.

Type annotations help callers and future edits, but they do not replace runtime validation or tests.

## Bad

```python
from typing import Any, cast


def user_name(record: dict[str, Any]) -> str:
    return cast(str, record["name"])  # Hides invalid input instead of checking it.
```

```toml
[tool.pyright]
typeCheckingMode = "off" # Hides diagnostics across the project.
```

Do not silence a diagnostic with a blanket `# type: ignore`, set the whole project to permissive mode to make a local change pass, or turn on strict checking for an unrelated legacy tree. Prefer fixing the type, adding a narrow check, or using a specific suppression with a short reason when an external limitation makes it necessary.

## Good

For a uv project with a `src` package and a local `.venv`, this is a minimal valid configuration. Set `pythonVersion` to the version the project supports, not automatically to the newest version.

```toml
[tool.pyright]
include = ["src"]
pythonVersion = "3.11"
venvPath = "."
venv = ".venv"
```

```sh
uv add --dev pyright
uv sync --locked
uv run --locked pyright
```

Keep annotations specific where they clarify interfaces or let the checker catch errors. Narrow a value at the point where it enters the program, and use a targeted ignore only when the checker cannot express a known-valid case:

```python
def user_name(record: dict[str, object]) -> str:
    value = record.get("name")
    if isinstance(value, str):
        return value
    raise ValueError("name must be a string")
```

## Configuration and caveats

- Pyright discovers `pyrightconfig.json` or a `[tool.pyright]` table in `pyproject.toml`. If both exist, `pyrightconfig.json` takes precedence. Relative paths are resolved from the configuration file.
- Configure `pythonVersion` to the project's supported syntax and runtime target. Configure `venvPath` and `venv` when needed so local imports resolve from the intended environment. Do not add a platform or version value based on the machine running the check if the project targets something else.
- In a multi-package repository, check the applicable project configuration and target paths. Avoid widening `include` or changing diagnostics outside the files relevant to the task.
- A clean static check does not prove runtime behavior. Run the relevant tests when the task calls for verification.
