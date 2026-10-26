# tool-uv

> Prefer `uv` for project dependencies, virtual environments, and project commands.

## Default

Use uv for new projects. In uv-managed projects, use `uv add`, `uv sync`, and `uv run` instead of direct `pip` installation or manual virtual-environment activation.

Use `uv init` only to start a new project. Preserve an existing toolchain such as Poetry, Hatch, PDM, or pip unless the user asks to migrate it.

## Why it matters

Project commands keep dependency metadata, the lockfile, and the environment aligned. `uv run` checks and syncs the project before it runs a command. `uv.lock` records resolved versions and belongs in version control for a uv-managed project.

## Bad

```sh
# In an existing Poetry project, silently switch managers.
uv init

# Install into whichever Python happens to be active.
pip install requests

# Update all locked packages without a request to upgrade them.
uv lock --upgrade
```

## Good

```sh
# Add a runtime dependency to a uv-managed project.
uv add requests

# Add a development tool, then run it through the project environment.
uv add --dev pyright
uv run pyright

# Recreate the environment from project metadata and the lockfile.
uv sync --locked
uv run --locked pytest
```

Use `uv add` only when a dependency change is part of the requested work. It updates `pyproject.toml`, `uv.lock`, and the environment.

Use `uv sync` when you need to install the declared dependencies. In CI, `uv sync --locked` and `uv run --locked ...` fail if the lockfile needs an update instead of silently changing it. Ordinary `uv sync` or `uv run` may update a stale lockfile, so inspect the diff when running those commands in a repository with pending dependency edits.

## Caveats

- `uv sync` is exact by default and removes packages that are not declared for the selected groups. Do not use it to preserve undocumented packages installed manually into `.venv`.
- `uv.lock` is managed by uv. Do not edit it by hand. Do not run upgrade flags unless updating versions is in scope.
- A standalone script or a project that already uses another package manager may have a different appropriate workflow. Follow its metadata and existing instructions.
- `uv run --with ...` is suitable for a one-off tool. Add a dependency to the project only when the project should retain it.
