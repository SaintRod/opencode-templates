# Python

## package management: uv
source: `https://pydevtools.com/handbook/how-to/how-to-configure-claude-code-to-use-uv/`

Use uv exclusively for Python package management in this project.
All Python dependencies **must be installed, synchronized, and locked** using uv.
Never use pip, pip-tools, poetry, or conda directly for dependency management.

Useful commands:

- Install dependencies: `uv add <package>`
- Remove dependencies: `uv remove <package>`
- Sync dependencies: `uv sync`
- Run a Python script with `uv run <script-name>.py`
- Run Python tools like Pytest with `uv run pytest`
- Launch a Python repl with `uv run python`

## formatting: ruff via uv

- format code using: `uvx ruff format .`

## plotting: plotnine

- module name: `plotnine`
- installation command: `'plotnine[extra]'`
- docs: `https://plotnine.org/reference/`

## referencing paths: pyhere

- module name: `pyhere`
- Use pyhere.here to reference relative paths
- `from pyhere import here`
- the result of `here()` is a string
- examples:
	- `here()` = project root
	- `here("data", "input")` = project-root/data/input
	- `here("data", "output", "results.arrow")` = project-root/data/output/results.arrow

## tables: great-tables

- module name: `great-tables`
- docs: `https://posit-dev.github.io/great-tables/articles/intro.html`

## CLI: docopt

- module name: `docopt-ng`
- docs: `https://github.com/jazzband/docopt-ng`

```{python}
"""Naval Fate.

Usage:
  naval_fate.py ship new <name>...
  naval_fate.py ship <name> move <x> <y> [--speed=<kn>]
  naval_fate.py ship shoot <x> <y>
  naval_fate.py mine (set|remove) <x> <y> [--moored | --drifting]
  naval_fate.py (-h | --help)
  naval_fate.py --version

Options:
  -h --help     Show this screen.
  --version     Show version.
  --speed=<kn>  Speed in knots [default: 10].
  --moored      Moored (anchored) mine.
  --drifting    Drifting mine.

"""
from docopt import docopt

if __name__ == "__main__":
    argv = ["ship", "Guardian", "move", "100", "150", "--speed=15"]
    arguments = docopt(__doc__, argv)
    print(arguments)
```

## data file formats: apache arrow

- module name: `pyarrow`
- preferred format: parquet
- `import pyarrow.parquet as pq`
- write: `pq.write_table(df, "file.parquet")`
- read: `pq.read_table("file.parquet")`

## data analysis: ibis

- module name: `ibis-framework`
- prefered backend: `duckdb`
- installation command: `'ibis-framework[duckdb]'`
- docs: `https://ibis-project.org/backends/duckdb`

