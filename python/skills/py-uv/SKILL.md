---
name: py-uv
description: >
  Manage Python package dependencies using uv for fast, reliable dependency management.
  Use when initializing projects, installing packages, or maintaining package consistency.
  Covers: (1) Initializing projects with uv init, (2) Installing packages with uv add,
  (3) Removing packages with uv remove, (4) Syncing dependencies with uv sync,
  (5) Running Python scripts and tools with uv run. Always use uv exclusively; never use pip,
  pip-tools, poetry, or conda directly for dependency management.
---

# uv

Use `uv` for Python package management. uv is fast, reliable, and manages dependencies
with a uv.lock file for reproducibility.

## Project Initialization

Initialize a new project (ask user for Python version):

```bash
# Example: uv init --no-package --python 3.12
uv init --no-package --python <VERSION>
```

This creates:
- `pyproject.toml` - Project configuration
- `uv.lock` - Locked dependency versions
- `.venv/` - Virtual environment

## Installing Packages

Add packages to the project:

```bash
# Single package
uv add pandas

# Multiple packages
uv add numpy scipy matplotlib

# Development dependency
uv add --dev pytest
```

## Removing Packages

Remove packages from the project:

```bash
# Single package
uv remove pandas

# Multiple packages
uv remove numpy scipy
```

## Syncing Dependencies

Synchronize installed packages with the lock file:

```bash
uv sync
```

Run this after cloning a project or when the lock file changes.

## Running Code

Execute Python scripts and tools:

```bash
# Run a Python script
uv run script.py

# Run with arguments
uv run script.py --input data.csv

# Run Python REPL
uv run python

# Run tools (pytest, black, etc.)
uv run pytest
uv run black .
```

## Must-Follow Rules

1. **Always use uv exclusively**: Never use pip, pip-tools, poetry, or conda directly.
   Use `uv add` instead of `pip install`.

2. **Initialize with --no-package**: Use `uv init --no-package` to create a project
   without building a package.

3. **Ask for Python version**: When initializing, ask the user which Python version
   to use (e.g., 3.11, 3.12).

4. **Use uv run for execution**: Always use `uv run` to execute Python scripts or
   tools; never activate the virtual environment manually.

5. **Sync after cloning**: Run `uv sync` after cloning a project with an existing
   `uv.lock` file.

## Example

```bash
#!/bin/bash

# Initialize project (user specifies Python version)
uv init --no-package --python 3.12

# Install packages
uv add pandas numpy

# Run script
uv run analysis.py

# Sync dependencies (after cloning)
uv sync
```
