---
name: r-box
description: >
  Import R packages and modules using the box package for explicit namespace management.
  Use when importing packages to avoid polluting the global environment and to make dependencies clear.
  Covers: (1) Importing and attaching specific functions using bracket syntax, (2) Creating aliases for
  long package names, (3) Handling name conflicts between packages with function aliases, (4) Importing
  local modules via relative paths, (5) Accessing documentation with box::help(). Default to importing
  and attaching specific functions; only import without attaching when importing >10 functions.
---

# box

Use `box::use()` to import R packages with explicit namespace control.
Default to importing and attaching specific functions.
Use aliases for long package names.

## Basic Syntax

```r
box::use(
    dplyr[arrange, filter, mutate, select],
    tbl = tibble,
    purrr,
)
```

## Import Patterns

### Import and attach specific functions (preferred)

List functions in brackets to attach them directly:

```r
box::use(
    dplyr[arrange, filter, mutate, select],
    tidyr[pivot_longer, pivot_wider],
)

# Usage: call directly
filter(mtcars, mpg > 20) |>
    select(mpg, cyl, hp)
```

### Import with alias

Use aliases for long package names:

```r
box::use(
    dt = data.table[fread, as.data.table],
    mb = microbenchmark[microbenchmark],
)

# Usage: dt$fread(), mb$microbenchmark()
dt$fread("data.csv")
```

### Import without attaching (>10 functions)

Only import without attaching when importing many functions (>10) to save space:

```r
box::use(
    stringr,  # >10 functions needed, access via stringr$function
)

# Usage: stringr$str_detect(), stringr$str_replace()
texts |> purrr$map_chr(~ stringr$str_to_upper(.x))
```

### Handle name conflicts

When two packages export the same function, alias one:

```r
box::use(
    dplyr[filter, select],
    stats[st_filter = filter],
)

filter(mtcars, mpg > 20)        # dplyr::filter
st_filter(some_ts, c(0, 1))    # stats::filter
```

### Accessing help documentation

Use `box::help()` to view documentation for imported packages:

```r
box::use(dplyr)

# View help for dplyr package
box::help(dplyr)

# View help for specific function
box::help(dplyr$filter)
```

**Note:** Standard `help("filter", "dplyr")` often does not work with box-imported namespaces. Use `box::help()` instead.

## Local Modules

Import R scripts using relative paths:

```r
box::use(
    src/data/cleaning[load_data, clean_names],
    src/utils/helpers[string_utils],
)

load_data("raw.csv") |> clean_names()
```

## Best Practices

1. **Import and attach specific functions**: List functions in brackets `[func1, func2]`
2. **Sort functions A-Z**: Within brackets, list functions alphabetically `[arrange, filter, mutate, select]`
3. **Alias long names**: `dt = data.table` instead of typing full name repeatedly
4. **Import without attaching only when >10 functions needed**: Use `pkg$function()` to save space
5. **Sort A-Z**: Keep imports sorted alphabetically
6. **Trailing commas**: Always include trailing comma on last entry

## Example

```r
#!/usr/bin/env Rscript

box::use(
    dplyr[arrange, filter, group_by, mutate, select, summarise],
    ggplot2[aes, geom_point, ggplot, ggsave],
    here[here],
    purrr[map, map_dbl, reduce],
    stringr,  # Many functions, keep namespaced
)

data <- stringr$str_to_upper(c("a", "b", "c"))
result <- map(data, ~ .x)
```
