---
name: py-ruff
description: >
  Format and lint Python code using ruff via uv. Use when formatting code, checking for style
  violations, or linting Python files. Covers: (1) Formatting code with ruff format,
  (2) Linting code with ruff check, (3) Listing installed Python versions.
  Configuration is handled automatically by uv via pyproject.toml.
---

# ruff

Use `ruff` for Python code formatting and linting. ruff is extremely fast and replaces
multiple tools (black, flake8, isort, pydocstyle).

## Formatting Code

Format all Python files in the current directory:

```bash
uvx ruff format .
```

Format a specific file:

```bash
uvx ruff format script.py
```

Check formatting without making changes:

```bash
uvx ruff format --check .
```

## Linting Code

Check code for style violations and errors:

```bash
# Check all files
uvx ruff check .

# Check specific file
uvx ruff check script.py

# Show all enabled rules
uvx ruff linter
```

## Checking Python Versions

List installed Python versions:

```bash
uv python list
```

## Must-Follow Rules

1. **Always use uvx to run ruff**: Use `uvx ruff`, never `ruff` directly.

2. **Format before committing**: Run `uvx ruff format .` before committing code.

3. **Fix linting errors**: Address all ruff check errors before committing.

4. **Let uv handle configuration**: Configuration goes in pyproject.toml; don't create
   separate .ruff.toml files.

## Example

```bash
#!/bin/bash

# Format all code
uvx ruff format .

# Check for linting errors
uvx ruff check .

# Check Python versions
uv python list
```
