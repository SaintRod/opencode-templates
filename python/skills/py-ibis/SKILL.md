---
name: py-ibis
description: >
  Perform data analysis using Ibis with DuckDB backend. Use when writing
  database-agnostic analytical queries that compile to efficient SQL.
  Covers: (1) Creating table references, (2) Building queries with Ibis
  expressions, (3) Using selectors for column operations, (4) Executing
  and materializing results.
---

# ibis

Use `ibis` for database-agnostic data analysis.
Ibis compiles expressions to efficient SQL (via DuckDB by default).

**Note:** Install `ibis-framework[duckdb]` using the py-uv skill.

## Imports

```python
import ibis
import ibis.selectors as s
from ibis import _
```

## Basic Usage

```python
import ibis
from ibis import _

# Connect to DuckDB
con = ibis.duckdb.connect("mydb.duckdb")

# Create table reference
t = con.table("my_table")

# Build query
result = (
    t
    .filter(_.value > 100)
    .select(_.id, _.value)
    .order_by(_.value.desc())
)

# Execute and get results
df = result.execute()
```

## Common Patterns

### Select columns

```python
import ibis
from ibis import _

# Select specific columns
result = t.select(_.id, _.name, _.value)

# Select with expressions
result = t.select(
    _.id,
    value_squared=_.value * _.value
)
```

### Filter rows

```python
import ibis
from ibis import _

# Simple filter
result = t.filter(_.value > 100)

# Multiple conditions
result = t.filter(
    (_.value > 100) & (_.category == "A")
)
```

### Aggregate

```python
import ibis
from ibis import _

# Group by and aggregate
result = (
    t
    .group_by(_.category)
    .aggregate(
        mean_value=_.value.mean(),
        count=_.count()
    )
)
```

### Use selectors

```python
import ibis.selectors as s

# Select numeric columns only
result = t.select(s.numeric())

# Select columns starting with 'sales'
result = t.select(s.startswith("sales"))
```

### Join tables

```python
import ibis
from ibis import _

# Simple join on column name
result = ratings.join(movies, "movieId", how="left")

# Join with explicit condition
result = ratings.join(
    movies,
    ratings.movieId == movies.movieId
)

# Self-join (use .view() to create distinct table reference)
movie_tags = tags["movieId", "tag"]
view = movie_tags.view()
result = movie_tags.join(
    view,
    movie_tags.movieId == view.movieId
)

# Complex join condition with tuple
result = movie_tags.join(
    view,
    [
        movie_tags.movieId != view.movieId,
        (_.tag.lower(), lambda t: t.tag.lower()),
    ]
)
```

## Interactive Mode

Control whether Ibis prints query information to stdout:

```python
import ibis

# Enable interactive mode (prints query info)
ibis.options.interactive = True

# Disable interactive mode (quiet)
ibis.options.interactive = False
```

Set to `True` for debugging and development; set to `False` for production scripts.

## Must-Follow Rules

1. **Import the underscore**: Always include `from ibis import _` for
   expression building.

2. **Use selectors**: Import and use `ibis.selectors` for column selection
   patterns.

3. **Wrap multiline queries in parentheses**: Use parentheses for readable
   multiline expression chains.

4. **Call .execute() to materialize**: Expressions are lazy; call execute()
   to get results.

5. **Use DuckDB backend**: This skill assumes DuckDB; other backends may
   have different syntax.

## Example

```python
#!/usr/bin/env python3

import ibis
import ibis.selectors as s
from ibis import _

# Connect to database
con = ibis.duckdb.connect("analytics.duckdb")

# Reference table
t = con.table("sales")

# Build analysis query
result = (
    t
    .filter(_.date >= "2024-01-01")
    .group_by(_.region, _.category)
    .aggregate(
        total_sales=_.amount.sum(),
        avg_amount=_.amount.mean(),
        transaction_count=_.count()
    )
    .order_by(_.region, _.total_sales.desc())
)

# Execute and get DataFrame
df = result.execute()
```
