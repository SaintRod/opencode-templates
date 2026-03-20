---
name: r-csv
description: >
  Read and write CSV files using the vroom package for fast, parallel processing.
  Use when importing or exporting tabular data in CSV or TSV format. Covers: (1) Basic reading
  with vroom(), (2) Reading with column selection and type specification, (3) Reading specific rows
  with skip and n_max parameters, (4) Writing with vroom_write(), (5) Writing tab-delimited files,
  (6) Appending to existing files. Supports compact type codes (c=character, i=integer, d=double, etc.)
  and both cols() and cols_only() specifications. Always use vroom instead of read.csv/write.csv for
  consistency and performance.
---

# CSV Import/Export with vroom

Use `vroom` for fast CSV reading and writing with parallel processing.

## Basic read
`vroom("data.csv")`

## Reading with options
```r
vroom(
    "data.csv",
    delim = ",",
    col_names = TRUE,
    col_types = NULL,
    col_select = NULL,
    skip = 0,
    n_max = Inf,
    na = c("", "NA"),
    quote = "\"",
    comment = "",
    trim_ws = TRUE,
    show_col_types = FALSE,
)
```

## Basic write
`vroom_write(data, "output.csv")`

## Writing with options
```r
vroom_write(
    data,
    "output.csv",
    delim = ",",
    na = "NA",
    append = FALSE,
    col_names = !append,
    escape = "double",
    quote = "needed",
    bom = FALSE,
)
```

## Common Use Cases

### Read with column selection

```r
data <- vroom(
    "large_file.csv",
    col_select = c(id, name, value),
    show_col_types = FALSE,
)
```

### Read specific rows

```r
# Skip first 5 rows, read next 1000
data <- vroom(
    "data.csv",
    skip = 5,
    n_max = 1000,
    col_names = FALSE,
)
```

### Specify column types

One of NULL, a cols() specification, or a string.

If NULL, all column types will be inferred from guess_max rows of the input, interspersed throughout the file. This is convenient (and fast), but not robust. If the guessed types are wrong, you'll need to increase guess_max or supply the correct types yourself.

Column specifications created by list() or cols() must contain one column specification for each column. If you only want to read a subset of the columns, use cols_only().

Alternatively, you can use a compact string representation where each character represents one column:

- c = character
- i = integer  
- I = big integer
- n = number
- d = double
- l = logical
- f = factor
- D = date
- T = date time
- t = time
- ? = guess
- _ or - = skip

By default, reading a file without a column specification will print a message showing the guessed types. To suppress this message, set show_col_types = FALSE.

```r
data <- vroom(
    "data.csv",
    col_types = cols(
        id = col_character(),
        date = col_date(format = "%Y-%m-%d"),
        amount = col_double(),
    ),
)

data <- vroom(
    "data.csv",
    col_types = c(cDd),
)
```

### Write tab-delimited

One or more characters used to delimit fields within a file. If NULL the delimiter is guessed from the set of c(",", "\t", " ", "|", ":", ";").

```r
vroom_write(
    data,
    "output.tsv",
    delim = "\t",
)
```

### Append to existing file

```r
vroom_write(
    new_data,
    "existing.csv",
    append = TRUE,
)
```

## Must-Follow Rules

1. **Always use vroom for CSV**: Never use `read.csv()` or `write.csv()` - vroom is faster and more consistent.

2. **Hide column types in scripts**: Set `show_col_types = FALSE` in production code to avoid console output clutter.

## Complete Example

```r
#!/usr/bin/env Rscript

# Read data
raw_data <- vroom(
    "data/input.csv",
    show_col_types = FALSE,
)

# Process
clean_data <- raw_data |
    subset(!is.na(value))

# Write results
vroom_write(
    clean_data,
    "output/results.csv",
    na = "",
)
```
