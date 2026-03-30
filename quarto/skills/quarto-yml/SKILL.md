---
name: quarto-yml
description: >
  Construct complete and effective _quarto.yml configuration files for the Quarto publishing system.
  Use when creating new Quarto projects, configuring existing projects, or when the user asks about _quarto.yml options.
  Covers: (1) Project configuration (type, output-dir, render), (2) Website settings (navbar, footer, dark mode),
  (3) HTML format options (TOC, code folding, custom CSS/JS), (4) Execute options (freeze, caching),
  (5) Bibliography with BibTeX, (6) Validation and common pitfalls, (7) Accessibility tips,
  (8) Brand integration with _brand.yml.
---

# Constructing _quarto.yml Files

This skill guides you through creating effective _quarto.yml configuration files for Quarto projects.

## 1. Project Configuration

Ask the user the following questions:

### Project Type
Ask: "What type of Quarto project is this?"
Options: `website`, `book`, `presentation`, `article`, `default`

If not specified or user is unsure, use `default`.

### Deployment Target
Ask: "Will the rendered content be deployed (e.g., to GitHub Pages, GitLab Pages)?"

- If deploying to **GitHub**: set `output-dir: docs`
- If deploying to **Gitlab**: set `output-dir: public`
- If not deploying: omit `output-dir`

### Render Scope
Ask: "Do you want to render specific content from the repo, or everything?"

If **specific content**, use the `render:` option with a list:
```yaml
project:
  type: website
  render:
    - index.qmd           # render file in root
    - posts/X.qmd        # render specific file in posts directory
    - posts/             # render all files in posts directory
    - "!AGENTS.md"       # explicitly ignore this file
```

## 2. Website Settings (when project type is website)

Ask the following questions:

### Title
Ask: "What is the title of your website?"
If not provided, generate a succinct but clear title based on the project context.

### Site URL
Ask: "What is the site URL?" (e.g., https://example.com)
If provided, add `site-url: "URL"`. Otherwise, omit.

### Favicon
Ask: "Do you have a favicon?" (path to favicon image)
If provided, add `favicon: "path/to/favicon.png"`. Otherwise, omit.

### Description
Ask: "What is a brief description of your website?"
If not provided, generate a succinct description that is clear but short.

### Search
Ask: "Should search be enabled?" (true/false)

### Bread Crumbs
Always set `bread-crumbs: true`.

### Page Footer
Ask: "What text should appear in the footer?" (e.g., your name or organization)

Always set `background: dark`. Use the current year for yyyy. If user provides text, use it; otherwise omit the left field or use a generic placeholder:

```yaml
page-footer:
  background: dark
  left: "© 2026-Present Your Name"    # replace "Your Name" with user input, use current year
```

### Navigation Bar (Navbar)

#### Social Links
Ask: "What social links should appear in the navbar?"
- LinkedIn URL (if provided)
- GitHub URL (if provided)
- GitLab URL (if provided)

For each social link, always add `target: _blank` to open in new tab.

#### Navbar Options
Ask the user about these optional navbar settings:

- **Logo**: Path to logo image (optional). Use `logo: false` to suppress brand logo.
- **Background color**: "primary", "secondary", "success", "danger", "warning", "info", "light", "dark", or hex color (optional)
- **Pinned**: Set to `true` to always show navbar (default: false, uses headroom.js)
- **Collapse**: Set to `true` to collapse navbar into hamburger menu on narrow screens (default: true)
- **Collapse breakpoint**: "sm", "md", "lg", "xl", or "xxl" (default: "lg")

```yaml
navbar:
  logo: "images/logo.png"              # optional
  background: dark                     # optional
  pinned: true                         # optional
  collapse: true                       # optional
  collapse-below: lg                   # optional
  tools:
    - icon: linkedin
      href: "https://linkedin.com/in/username"
      target: _blank
    - icon: github
      href: "https://github.com/username"
      target: _blank
    - icon: gitlab
      href: "https://gitlab.com/username"
      target: _blank
```

### Website Section Template
```yaml
website:
  title: "Your Title"
  site-url: "https://example.com"        # if provided
  favicon: "static/favicon.png"          # if provided
  description: "Your description"        # required
  bread-crumbs: true
  search: true                            # or false
  page-footer:
    background: dark
    left: "© 2026-Present Your Name"
  navbar:
    tools:
      - icon: linkedin
        href: "https://linkedin.com/in/username"
        target: _blank
      - icon: github
        href: "https://github.com/username"
        target: _blank
      - icon: gitlab
        href: "https://gitlab.com/username"
        target: _blank
```

## External Links Rule
**IMPORTANT:** Anytime a URL is referenced in _quarto.yml (in navbar, footer, or any other section), always add `target: _blank` to open links in a new tab/page.

## 3. Global Settings

### Language
Always set `lang: en-US` at the top level (not nested under any key).

### Author
Ask: "What is your name?" (for the author field)
If provided, add to metadata. Otherwise, omit.

```yaml
lang: en-US

author: "Your Name"      # if provided
```

## 4. HTML Format Options

Set these defaults for HTML output:
```yaml
format:
  html:
    toc: true
    toc-location: right
    toc-title: "Table of Contents"
    link-external-icon: true
    link-external-newwindow: true
    code-fold: true
    code-summary: "Unhide"
    code-copy: true
    embed-resources: true
    number-sections: true
```

Ask: "Should search be enabled in the HTML format?" (true/false)
If enabled, add `search: true` under `format: html:`.

### Dark/Light Mode Toggle
Ask: "Should dark/light mode toggle be enabled?"
- If yes: Add `appearance: light` (shows toggle, defaults to light) or `appearance: dark` (defaults to dark)
- If no: Omit this option

```yaml
format:
  html:
    # ... other options ...
    appearance: light
```

### Custom CSS/JS Files
Check if custom CSS or JS files exist in the repository:
```bash
ls *.css
ls *.js
ls styles/
```

If CSS/JS files exist or user requests custom formatting:
- Use `css:` to include CSS files
- Use `include-in-header:` to include custom JS or other header content

```yaml
format:
  html:
    # ... other options ...
    css: ["styles/custom.css", "styles/theme.css"]
    include-in-header: "custom-js.html"
```

For simple inline additions, you can use:
```yaml
format:
  html:
    include-in-body: |
      <script>console.log("Custom JS");</script>
```

## 5. Execute Options

Set these defaults:
```yaml
execute:
  eval: true
  echo: false
  warning: false
  error: false
  freeze: false
  daemon: false
```

Ask: "Should freeze be enabled?" (true/false)
- `freeze: true` - cache computed results and only re-render when code chunks change
- `freeze: false` (default) - always re-render all code

## 6. Bibliography and References

Ask: "Do you have a bibliography/references file?" (path to .bib file)

If **yes** and the file exists:
```yaml
bibliography: references.bib
```

If **yes** but the **file does not exist**:
Ask the user to provide the reference details and help them create `references.bib` directly. Use BibTeX format.

If **no**: omit bibliography option.

### BibTeX Format

Use this format for references in `references.bib`:

```bibtex
@article{davenport2012,
  title={Data Scientist: The Sexiest Job of the 21st Century},
  author={Davenport, Thomas H. and Patil, D. J.},
  journal={Harvard Business Review},
  year={2012},
  month={October},
  url={https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century},
  note={Accessed on October 28, 2024}
}

@book{davenport2007COA,
  author    = {Thomas H. Davenport and Jeanne G. Harris},
  title     = {Competing on Analytics: The New Science of Winning},
  year      = {2007},
  publisher = {Harvard Business Review Press},
  address   = {Boston, MA},
  isbn      = {9781422103326}
}
```

Common entry types: `@article`, `@book`, `@inproceedings`, `@incollection`, `@techreport`, `@misc`, `@phdthesis`, `@mastersthesis`.

## 7. Validation

After creating or modifying `_quarto.yml`, always validate the configuration:

```bash
quarto check all
```

This checks:
- Quarto installation and dependencies
- Project structure
- YAML syntax validity
- Required files present
- Bibliography file validity (if configured)
- Code execution requirements

Fix any errors reported.

## 8. Common Pitfalls

### YAML Syntax
- **Indentation**: YAML requires consistent spaces (not tabs). Use 2 spaces per level.
- **Quotes**: Use quotes for strings with special characters (`:`, `#`, `@`, etc.)
- **Booleans**: Use `true`/`false` (lowercase), not `True`/`False`/`yes`/`no`
- **Null values**: Use `~` or omit the key, not `null` or `none`

### Paths
- **Relative paths**: Paths are relative to project root
- **Forward slashes**: Always use `/` even on Windows
- **File existence**: Verify referenced files exist before rendering

### Common Errors
- `Duplicate key` - Check for duplicate entries
- `mapping values are not allowed` - Check indentation
- `expected '<stream>', found` - Check for syntax errors
- Missing required fields for navbar icons

## 9. Accessibility Tips

### Navigation
- Ensure navbar is keyboard navigable
- Use meaningful link text (avoid "click here")
- Add `aria-label` to icons and buttons without text

### Search
- Ensure search is keyboard accessible

## 10. Brand Integration

Check if a `_brand.yml` file exists in the project directory:
```bash
ls _brand.yml
```

If `_brand.yml` exists, add:
```yaml
brand: _brand.yml
```

## Example: Complete _quarto.yml

```yaml
project:
  type: website
  output-dir: docs
  render:
    - index.qmd
    - posts/
    - "!AGENTS.md"

lang: en-US

website:
  title: "Portfolio"
  site-url: "https://sr-portfolio.thealgo.group"
  favicon: static/favicon.png
  description: "My portfolio"
  bread-crumbs: true
  search: true
  page-footer:
    background: dark
    left: "© 2026-Present Your Name"
  navbar:
    tools:
      - icon: linkedin
        href: "https://linkedin.com/in/username"
        target: _blank
      - icon: github
        href: "https://github.com/username"
        target: _blank
      - icon: gitlab
        href: "https://gitlab.com/username"
        target: _blank

format:
  html:
    toc: true
    toc-location: right
    toc-title: "Table of Contents"
    link-external-icon: true
    link-external-newwindow: true
    code-fold: true
    code-summary: "Unhide"
    code-copy: true
    embed-resources: true
    number-sections: true
    search: true

execute:
  eval: true
  echo: false
  warning: false
  error: false
  freeze: true
  daemon: false

bibliography: references.bib

brand: _brand.yml
```

## Additional Useful Options

For **presentations**, add:
```yaml
format:
  revealjs:
    slide-number: true
    chalkboard: true
    footnotes: true
```

For **books**, add:
```yaml
book:
  chapters:
    - index.qmd
    - part: "Part I"
      chapters:
        - intro.qmd
        - methods.qmd
```

- **`date`** - Publication date (e.g., `date: "2024-01-15"`)
- **`metadata`** - Additional metadata key-value pairs
