---
name: r-here
description: >
  Reference paths relative to the project root using the here() function from the here package.
  Use when constructing file paths that need to work across different working directories or
  machines. Covers: (1) Referencing the project root, (2) Constructing paths to subdirectories,
  (3) Referencing specific files. Always use here() instead of file.path() for project-relative
  paths to ensure portability and reproducibility.
---

# here

Use `here()` to construct paths relative to the project root. The function automatically detects
your project root using reasonable heuristics and constructs absolute paths from relative
components.

## Patterns

Create path variables to improve readability and enable reuse:

```r
# Simple paths
path_data <- here("data")
path_output <- here("output")
path_figures <- here("figures")

# Nested paths - create parent variables first, then build child paths
path_data_raw <- here(path_data, "raw")
path_data_processed <- here(path_data, "processed")

# File paths - reference parent directories
path_data_raw_input <- here(path_data_raw, "input.csv")
path_data_processed_output <- here(path_data_processed, "output.csv")
path_output_results <- here(path_output, "results.csv")
```

## Must-Follow Rules

1. **Always use here() for project paths**: Never use hardcoded absolute paths like `/home/user/project/data/file.csv`

2. **Always use here() instead of file.path()**: here() ensures paths are relative to project root regardless of working directory

3. **Keep paths relative**: Pass path components as separate arguments separated by commas

4. **Never assume working directory**: Scripts should work regardless of which directory they are run from

5. **Create variables using nested here()**: Save here() output to variables rather than nesting within other code. Build paths by creating parent directory variables first, then constructing child paths by nesting here() calls. This eliminates repetition, improves readability, and makes refactoring easier.
