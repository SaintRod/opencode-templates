---
name: py-plotnine
description: >
  Create publication-quality plots using plotnine, a Python implementation of ggplot2.
  Use when creating statistical visualizations, exploring data, or generating figures.
  Covers: (1) Creating basic plots with ggplot(), (2) Adding aesthetics and geoms,
  (3) Customizing themes and scales, (4) Saving plots. Installation uses 'plotnine[extra]'
  for extended functionality.
---

# plotnine

Use `plotnine` for creating publication-quality statistical graphics. plotnine implements
the grammar of graphics (same as R's ggplot2) in Python.

**Note:** Install `plotnine[extra]` using the py-uv skill.

## Basic Usage

```python
from plotnine import (
    ggplot,
    aes,
    geom_point,
    geom_line,
    geom_bar,
    theme_minimal,
    labs,
    ggsave,
)

# Create a scatter plot
(
    ggplot(data, aes(x="var1", y="var2"))
    + geom_point()
    + theme_minimal()
    + labs(title="My Plot", x="X Label", y="Y Label")
)
```

## Common Patterns

### Scatter plot

```python
from plotnine import ggplot, aes, geom_point, theme_minimal

(
    ggplot(data, aes(x="x_var", y="y_var"))
    + geom_point()
    + theme_minimal()
)
```

### Line plot

```python
from plotnine import ggplot, aes, geom_line

(
    ggplot(data, aes(x="x_var", y="y_var", group="group_var"))
    + geom_line()
)
```

### Bar plot

```python
from plotnine import ggplot, aes, geom_bar

(
    ggplot(data, aes(x="category", y="value"))
    + geom_bar(stat="identity")
)
```

### Saving plots

```python
from plotnine import ggsave

# Save to file
ggsave(plot, "output.png", dpi=300)
ggsave(plot, "output.pdf")
```

## Must-Follow Rules

1. **Import specific functions**: Import only what you need from plotnine modules.

2. **Use parentheses for multiline**: Wrap plot code in parentheses for readability.

3. **Use theme_minimal() as default**: Prefer theme_minimal() unless specifically needed.

4. **Save with high DPI**: Use dpi=300 when saving PNG files for publication quality.

## Example

```python
#!/usr/bin/env python3

from plotnine import (
    ggplot,
    aes,
    geom_point,
    geom_smooth,
    theme_minimal,
    labs,
    ggsave,
)

# Create plot
plot = (
    ggplot(data, aes(x="gdp_per_cap", y="life_exp"))
    + geom_point(alpha=0.5)
    + geom_smooth(method="lm")
    + theme_minimal()
    + labs(
        title="GDP vs Life Expectancy",
        x="GDP per capita",
        y="Life Expectancy",
    )
)

# Save plot
ggsave(plot, "gdp_life_exp.png", dpi=300)
```
