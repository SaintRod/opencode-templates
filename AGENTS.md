# Purpose

The purpose of this repo is to create useful skills for users.

# How and where to create SKILL.md files

Only create SKILL.md files withing the `language/skills/<name>/` directory

Skills files should only be named `SKILL.md`

Examples:

- `r/skills/<name>/SKILL.md`
- `py/skills/<name>/SKILL.md`
- `julia/skills/<name>SKILL.md`

If a `language/skills` path does not exist, create it via `mkdir -p language/skills/<name>/`

The `<name>` should be short but clear, for example `r-box` suggests the skill pertains to the R programming language and relates to the box library

# SKILL.md frontmatter

Each SKILL.md must start with YAML frontmatter. Only these fields are recognized:

name (required)
description (required)
license (optional)
compatibility (optional)
metadata (optional, string-to-string map)
Unknown frontmatter fields are ignored.

## Writing effective descriptions

The description field should be detailed and informative. Use the `>` YAML multi-line format to write comprehensive descriptions that help the system match user requests to the appropriate skill. An effective description should:

- Explain when to use the skill (trigger conditions)
- List specific actions or workflows covered
- Include key functions or features
- State any important rules or defaults
- Mention any unique conventions or patterns

Example:
```yaml
description: >
  Manage R package dependencies using renv to create isolated, reproducible environments.
  Use when initializing projects, installing packages, or maintaining consistency.
  Covers: (1) Initializing with renv::init(), (2) Installing with renv::install(),
  (3) Saving state with renv::snapshot(). Always use renv::install() instead of
  install.packages(). Commit renv.lock to version control.
```

# How to write an effective skill

- narrow scoped
- skills are succinct but clear, ensure nothing is ambigious
- use examples
