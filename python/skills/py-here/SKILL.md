---
name: py-here
description: >
  Reference paths relative to the project root using pyhere.here().
  Use when constructing file paths that need to work across different working directories.
  Covers: (1) Referencing the project root, (2) Constructing paths to subdirectories,
  (3) Referencing specific files. Similar to R's here package - create parent variables
  first, then build nested paths.
---

# pyhere

Use `pyhere.here()` to construct paths relative to the project root. The function finds
the project root using markers (like .git, pyproject.toml) and constructs absolute paths.

**Note:** Install `pyhere` using the py-uv skill.

## Return Type

The `here()` function returns a `pathlib.PosixPath` object:

```python
>>> from pyhere import here
>>> here()
PosixPath('/home/user/project/data')
>>> type(here())
<class 'pathlib.PosixPath'>
```

Note: This differs from R's here() which returns a string. If a string is required, use `str(here())`.

## Basic Usage

```python
from pyhere import here

# Get project root
project_root = here()

# Reference subdirectories
path_data = here("data")
path_data_raw = here("data", "raw")
path_output = here("output", "results")

# Reference specific files
input_file = here("data", "raw", "input.csv")
```

## Nested Paths

Build paths by nesting here() variables to avoid repetition:

```python
from pyhere import here

# Create parent directory variables
path_data = here("data")
path_output = here("output")

# Build nested paths from parent variables
path_data_raw = here(path_data, "raw")
path_data_cleaned = here(path_data, "cleaned")

# Build file paths from directory variables
path_data_raw_input = here(path_data_raw, "input.csv")
path_data_raw_survey = here(path_data_raw, "survey.csv")
```

## Common Patterns

### Data files

```python
from pyhere import here

# Create path variables
input_path = here("data", "raw", "survey.csv")
output_path = here("output", "processed.csv")

# Use the path variables
print(input_path)
print(output_path)
```

### Multiple directories

```python
from pyhere import here

# Create variables for directories
raw_dir = here("data", "raw")
clean_dir = here("data", "cleaned")
output_dir = here("output")

# Reference directories
print(raw_dir)
print(clean_dir)
```

## Must-Follow Rules

1. **Always use here() for project paths**: Never use hardcoded absolute paths.

2. **Create variables using nested here()**: Create parent directory variables first,
   then build child paths by nesting here() calls. This eliminates repetition.

3. **Import from pyhere**: Use `from pyhere import here`, not other import styles.

4. **Remember it returns PosixPath**: The result of here() is a pathlib.PosixPath object, not a string.

## Example

```python
#!/usr/bin/env python3

from pyhere import here

# Create parent directory variables
path_data = here("data")
path_output = here("output")

# Build nested paths from parent variables
path_data_raw = here(path_data, "raw")
path_data_processed = here(path_data, "processed")

# Build file paths from directory variables
path_data_raw_input = here(path_data_raw, "input.csv")
path_data_processed_output = here(path_data_processed, "output.csv")
path_output_results = here(path_output, "results.csv")

# Use paths
print(f"Input: {path_data_raw_input}")
print(f"Output: {path_output_results}")
```
