---
name: py-tables
description: >
  Create publication-ready tables using great-tables. Use when formatting dataframes
  into presentation-quality tables for reports, HTML output, or sharing. Supports both
  pandas and polars DataFrames. Covers: (1) Creating basic tables with GT(),
  (2) Adding headers and footers, (3) Formatting columns and cells,
  (4) Styling and theming tables.
---

# great-tables

Use `great_tables` to create publication-quality tables from pandas or polars DataFrames.

**Note:** Install `great-tables` using the py-uv skill.

## Basic Usage

```python
from great_tables import GT

# Create a basic table
table = GT(df)

# Show the table
table
```

## Common Patterns

### Adding headers

```python
from great_tables import GT

(
    GT(df)
    .tab_header(
        title="Table Title",
        subtitle="Table Subtitle"
    )
)
```

### Formatting columns

```python
from great_tables import GT, loc, style

(
    GT(df)
    .fmt_number(
        columns=["value", "price"],
        decimals=2
    )
    .fmt_percent(
        columns=["percentage"],
        decimals=1
    )
)
```

### Styling cells

```python
from great_tables import GT, loc, style

(
    GT(df)
    .tab_style(
        style=style.fill(color="lightgreen"),
        locations=loc.body(
            columns=["value"],
            rows=df["value"] > 100
        )
    )
)
```

### Adding footers

```python
from great_tables import GT

(
    GT(df)
    .tab_source_note("Source: Company data 2024")
    .tab_source_note("Note: Values in USD")
)
```

## Must-Follow Rules

1. **Chain methods for readability**: Use method chaining with parentheses for
   multiline table construction.

2. **Import selectively**: Import only the classes and functions you need from
   great_tables.

3. **Use fmt_* methods for formatting**: Always use built-in formatting methods
   rather than pre-formatting data.

4. **Add source notes**: Always include source notes for data provenance.

## Example

```python
#!/usr/bin/env python3

from great_tables import GT, loc, style

# Create and style table
table = (
    GT(df)
    .tab_header(
        title="Sales Summary",
        subtitle="Q1 2024"
    )
    .fmt_number(
        columns=["revenue", "profit"],
        decimals=0
    )
    .fmt_percent(
        columns=["margin"],
        decimals=1
    )
    .tab_style(
        style=style.fill(color="lightgreen"),
        locations=loc.body(
            columns=["profit"],
            rows=df["profit"] > 0
        )
    )
    .tab_source_note("Source: Sales Database")
)

# Display
table
```
