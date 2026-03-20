---
name: r-help
description: >
  Access R function documentation using help() with unquoted function names. Use when encountering
  function errors, unused argument warnings, deprecated warnings, or when needing to verify function
  signatures and parameters. Covers: (1) Reading function documentation, (2) Checking function arguments,
  (3) Verifying parameter names and defaults. Always use unquoted function names in help().
---

# help

Use `help()` to read R function documentation when encountering errors or warnings. Function names
should be unquoted (not in quotes).

## Basic Usage

```r
# Read documentation for a function
help(function_name)

# Examples
help(vroom)
help(filter)
help(here)
```

## When to Check Documentation

Check function documentation when you encounter:

- **Unused argument warnings**: Function doesn't recognize a parameter you're using
- **Deprecated warnings**: Function behavior or parameters have changed
- **Errors**: Function fails with argument-related errors
- **Unclear parameters**: Need to verify parameter names or expected values

## Example Scenarios

### Checking unused argument warnings

```r
# If you get: "unused argument (show_col_types = FALSE)"
# Check the documentation:
help(vroom)

# Look for the correct parameter name in the docs
```

### Verifying parameter names

```r
# Before using a function, verify the parameter names
help(vroom_write)

# Check what the 'na' parameter accepts
```

### Checking for deprecated functions

```r
# If you see deprecation warnings
help(summarise)

# Look for notes about parameter changes
```

## Must-Follow Rules

1. **Always use unquoted function names**: Use `help(vroom)`, never `help("vroom")` or `?vroom`

2. **Check docs when encountering errors**: When you see unused argument warnings or deprecated
   notices, immediately read the function documentation to verify current function signature

3. **Verify parameter names**: Before using function parameters, check the documentation to ensure
   parameter names haven't changed between versions

4. **Read documentation comprehensively**: Review the "Description" section for function overview, the "Arguments" section to see parameter names, types, and defaults, the "Value" section to understand return types and structure, and the "Examples" section to see correct usage patterns. This ensures complete understanding of function behavior and expected inputs/outputs.

5. **Use box::help() when base help() fails**: If `help()` returns an error or cannot find documentation for a function, and the function was imported via the box package, reference the r-box skill to learn how to use `box::help()` instead. The box package requires `box::help(package)` or `box::help(package$function)` for accessing documentation.

## Example

```r
#!/usr/bin/env Rscript

# You encounter an error with this code:
# vroom("data.csv", show_types = FALSE)
# Error: unused argument (show_types = FALSE)

# Check the documentation
help(vroom)

# Discover the correct parameter is show_col_types, not show_types
# Fix the code:
# vroom("data.csv", show_col_types = FALSE)
```
