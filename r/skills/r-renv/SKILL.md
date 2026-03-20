---
name: r-renv
description: >
  Manage R package dependencies using renv to create isolated, reproducible R environments.
  Use when initializing projects, installing packages, or maintaining package consistency across
  environments. Covers: (1) Initializing projects with renv::init(), (2) Installing packages with
  renv::install(), (3) Removing packages with renv::remove(), (4) Checking status with renv::status(),
  (5) Saving state with renv::snapshot(), (6) Restoring from lockfile with renv::restore(), (7) Updating
  packages with renv::update(). Always use renv::install() instead of install.packages() to maintain
  project isolation. Commit renv.lock to version control for reproducibility.
---

# renv

Use renv to create isolated, reproducible R package environments.
Each project has its own private library.

## Core Workflow

```r
# 1. Initialize
renv::init()

# 2. Install packages as needed
renv::install("dplyr")
renv::install(c("ggplot2", "purrr"))

# 3. Check status
renv::status()

# 4. Snapshot when ready
renv::snapshot()
```

## Main Functions

### Initialize project

Create a private library for the current project:

```r
renv::init()
```

Creates `renv/` directory and `renv.lock` file. Run once per project.

### Install packages

Install packages into the project's private library:

```r
# Single package
renv::install("dplyr")

# Multiple packages
renv::install(c("ggplot2", "purrr", "tidyr"))

# Specific version
renv::install("dplyr@1.1.0")

# From GitHub
renv::install("tidyverse/dplyr")
```

### Remove packages

Remove packages from the private library:

```r
# Single package
renv::remove("dplyr")

# Multiple packages
renv::remove(c("ggplot2", "purrr"))
```

### Check status

See which packages are installed vs recorded in lockfile:

```r
renv::status()
```

Shows:
- Installed but not recorded (need `snapshot()`)
- Recorded but not installed (need `restore()`)
- Out of sync versions

### Save state

Record current package versions to `renv.lock`:

```r
renv::snapshot()
```

Run after installing/removing packages to update the lockfile.

### Restore from lockfile

Install packages exactly as recorded in `renv.lock`:

```r
renv::restore()
```

Use when cloning a project or to reset to the recorded state.

**Note:** `renv::restore()` works best when the local R version matches the lockfile's major.minor version (e.g., 4.4.x). It may fail if versions differ significantly (e.g., lockfile is 4.4.1 but local is 4.5.2).

### Update packages

Update packages to latest versions:

```r
# Update single package
renv::update("dplyr")

# Update all packages
renv::update()

# Then snapshot the updates
renv::snapshot()
```

## Must-Follow Rules

1. **Always use `renv::install()`, never `install.packages()`**: Keeps packages in the project's private library, not the global library.

2. **Snapshot after changing packages**: Run `renv::snapshot()` after installing or removing packages to update `renv.lock`.

3. **Check `renv::status()` before committing**: Ensure lockfile matches installed packages.

4. **Commit `renv.lock` to version control**: This file records exact versions for reproducibility.

5. **Use `renv::restore()` when cloning**: After cloning a project with an existing `renv.lock`, run `renv::restore()` to install the exact recorded versions.

## Example Workflow

```r
# Initialize new project
renv::init()

# Install packages needed
renv::install("dplyr")
renv::install("ggplot2")

# Work on project...
library(dplyr)
library(ggplot2)

# Check what changed
renv::status()

# Save the state
renv::snapshot()

# Commit renv.lock to git
```

## Working with Lockfile

```r
# View lockfile without opening file
cat(readLines("renv.lock"), sep = "\n")

# See summary
renv::status()

# Restore exact versions from lockfile
renv::restore()
```
