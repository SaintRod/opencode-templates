---
name: r-rig
description: >
  Run R code using rig, the R installation manager. Use when executing R scripts to ensure
  the correct R version is used. Covers: (1) Running R scripts with rig run, (2) Passing
  command-line arguments to R scripts, (3) Checking the default R version. Always use rig
  instead of calling R directly to ensure version consistency with the project's renv.lock.
---

# rig

Use `rig` to run R code with a specific version. rig is installed at the system level and manages
multiple R versions.

## Running R Scripts

Execute an R script using a specific R version:

```bash
rig run -r 4.4.3 -f main.R
```

## Running Commands

Execute R code directly without a file using the `-e` flag:

```bash
# Run a single command
rig run -r 4.5.1 -e "help(function)"

# Run code that prints output
rig run -r 4.5.1 -e "print('Hello from R')"
```

Multiple commands can be executed by separating them with semicolons within the quoted string.

Use `-e` for quick checks, testing code snippets, or running documentation commands without creating a file.

## Passing Arguments

Pass arguments to the R script after `--`:

```bash
# Pass custom arguments
rig run -r 4.4.3 -f main.R -- --help
rig run -r 4.4.3 -f main.R -- -t TRUE
rig run -r 4.4.3 -f main.R -- --input data.csv --output results.csv
```

## Checking Default Version

Check which R version is currently set as default:

```bash
rig default
```

**Important:** Compare the output with the R version specified in `renv.lock`. If they differ,
alert the user before proceeding.

## Getting Help

View rig's help documentation to learn about available commands and options:

```bash
# General help
rig --help

# Help for a specific command
rig run --help
```

Use `rig --help` to discover other available commands and their options.

## Project Setup Checklist

Perform this check **once** when first working with an R project:

1. Check the default R version:
   ```bash
   rig default
   ```

2. Compare with the R version specified in `renv.lock`

3. **If versions differ**: Alert the user immediately, wait for instructions, and do nothing else until the user responds

4. **If versions match**: Proceed with running R code

**Important**: This check should only happen once per project session, not before every rig command.

## Must-Follow Rules

1. **Always use rig to run R code**: Never run `Rscript` or `R` directly. Always use `rig run` to ensure the correct version is used.

2. **Pass arguments after `--`**: When passing arguments to the R script, always place them after the `--` separator.

3. **Use the version from renv.lock**: When available, use the R version specified in `renv.lock` with the `-r` flag.

## Example

```bash
#!/bin/bash

# Check default R version
echo "Default R version:"
rig default

# Run the script with specific version
rig run -r 4.4.3 -f main.R

# Run with arguments
rig run -r 4.4.3 -f main.R -- --verbose
```
