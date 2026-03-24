---
name: py-parquet
description: >
  Read and write Parquet files using pyarrow. Use when working with columnar data
  formats for efficient storage and querying. Covers: (1) Reading Parquet files,
  (2) Writing DataFrames to Parquet, (3) Working with pyarrow Tables.
---

# pyarrow

Use `pyarrow` to work with Parquet files for efficient columnar data storage.

**Note:** Install `pyarrow` using the py-uv skill.

## Reading Parquet

```python
import pyarrow.parquet as pq

# Read to pyarrow Table
table = pq.read_table("data.parquet")

# Convert to pandas DataFrame (optional and only if requested or required)
df = table.to_pandas()
```

## Writing Parquet

```python
import pyarrow.parquet as pq

# Write from pyarrow Table
pq.write_table(table, "output.parquet")
```

## Common Patterns

### Read specific columns

```python
import pyarrow.parquet as pq

# Read only specific columns (memory efficient)
table = pq.read_table(
    "data.parquet",
    columns=["col1", "col2", "col3"]
)
```

### Read with filters

```python
import pyarrow.parquet as pq
import pyarrow.compute as pc

# Filter while reading
table = pq.read_table(
    "data.parquet",
    filters=[("year", ">=", 2020)]
)
```

### Write with compression

```python
import pyarrow.parquet as pq

# Write with Snappy compression (default)
pq.write_table(table, "output.parquet")

# Write with different compression
pq.write_table(
    table,
    "output.parquet",
    compression="zstd"
)
```

## Must-Follow Rules

1. **Use pyarrow.parquet**: Import as `import pyarrow.parquet as pq`.

2. **Read only needed columns**: Use the `columns` parameter to reduce memory
   usage.

3. **Let pyarrow handle types**: pyarrow infers types automatically; only
   specify when necessary.

4. **Use compression**: Accept the default Snappy compression or use zstd for
   better compression ratios.

## Example

```python
#!/usr/bin/env python3

import pyarrow.parquet as pq
import pyarrow as pa

# Read data (selective columns for memory efficiency)
raw_table = pq.read_table(
    "data/raw.parquet",
    columns=["id", "value", "date"]
)

# Process...
# (transform raw_table into processed_table)

# Write processed data
pq.write_table(
    processed_table,
    "output/processed.parquet",
    compression="zstd"
)
```
