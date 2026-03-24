---
name: py-cli
description: >
  Create command-line interfaces using docopt-ng. Use when building CLI tools
  that need argument parsing. Covers: (1) Writing docstrings with usage patterns,
  (2) Parsing arguments with docopt(), (3) Validating and handling arguments,
  (4) Error handling for invalid inputs.
---

# docopt

Use `docopt-ng` (imported as `docopt`) to create command-line interfaces by writing help text. docopt parses
the usage patterns from your docstring.

**Note:** Install `docopt-ng` using the py-uv skill.

## Basic Usage

```python
from docopt import docopt

# Define usage in docstring
"""My Tool

Usage:
  mytool.py <input> <output>
  mytool.py --version

Options:
  -h --help     Show this screen.
  --version     Show version.

"""

# Parse arguments
if __name__ == "__main__":
    arguments = docopt(__doc__)
    print(arguments)
```

## Common Patterns

### Positional arguments

```python
"""Usage:
  mytool.py <input> <output>
"""

arguments = docopt(__doc__)
input_file = arguments["<input>"]
output_file = arguments["<output>"]
```

### Optional arguments

```python
"""Usage:
  mytool.py [--verbose] [--limit=<n>]

Options:
  --verbose      Enable verbose output
  --limit=<n>    Limit to n items [default: 10]
"""

arguments = docopt(__doc__)
verbose = arguments["--verbose"]
limit = int(arguments["--limit"])
```

### Flags and choices

```python
"""Usage:
  mytool.py (--csv | --json) <file>

Options:
  --csv     Output as CSV
  --json    Output as JSON
"""

arguments = docopt(__doc__)
if arguments["--csv"]:
    format = "csv"
elif arguments["--json"]:
    format = "json"
```

## Validation and Error Handling

Validate arguments after parsing:

```python
from docopt import docopt
import os

arguments = docopt(__doc__)
input_file = arguments["<input>"]

# Validate file exists
if not os.path.exists(input_file):
    print(f"Error: File '{input_file}' not found")
    exit(1)

# Validate numeric values
if arguments["--limit"]:
    try:
        limit = int(arguments["--limit"])
        if limit <= 0:
            raise ValueError
    except ValueError:
        print("Error: --limit must be a positive integer")
        exit(1)
```

## Must-Follow Rules

1. **Write clear usage strings**: Be explicit about required vs optional arguments.

2. **Always validate inputs**: Check file existence, numeric ranges, etc. after
   parsing, not in docopt itself.

3. **Provide sensible defaults**: Use `[default: value]` syntax for optional args.

4. **Handle errors gracefully**: Print helpful error messages and exit with
   non-zero status on validation failures.

## Example

```python
#!/usr/bin/env python3
"""Data Processor

Usage:
  processor.py <input> <output> [--limit=<n>] [--verbose]
  processor.py --version

Options:
  -h --help       Show this screen.
  --version       Show version.
  --limit=<n>     Process only n rows [default: all].
  --verbose       Enable verbose output.

"""

from docopt import docopt
import os


def main():
    arguments = docopt(__doc__, version="1.0.0")
    
    input_file = arguments["<input>"]
    output_file = arguments["<output>"]
    verbose = arguments["--verbose"]
    
    # Validate input file exists
    if not os.path.exists(input_file):
        print(f"Error: Input file '{input_file}' not found")
        exit(1)
    
    # Validate limit if provided
    if arguments["--limit"]:
        try:
            limit = int(arguments["--limit"])
            if limit <= 0:
                raise ValueError
        except ValueError:
            print("Error: --limit must be a positive integer")
            exit(1)
    
    # Process...
    if verbose:
        print(f"Processing {input_file}...")


if __name__ == "__main__":
    main()
```
