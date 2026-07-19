# Tools

- If `rtk` is installed, it runs as an opencode hook and cannot be bypassed by agents

# Formatting and code style guide

- public modules, classes, and non-obvious functions should have succinct documentation when the language supports it
- code should be self-documenting, but when necessary add succinct comments
- when generating text, bullet points must not end with a period character

# Instruction precedence

- user instructions override project instructions
- project AGENTS.md instructions override global AGENTS.md instructions

# Worktree safety

- preserve unrelated worktree changes
- never revert, overwrite, or clean up user changes unless explicitly asked
- before committing, inspect status, diff, and recent commits
- stage only files relevant to the requested work

# Research

Prefer research before planning for ambiguous, architectural, risky, or domain-heavy work.
Skip research for small, obvious changes where a plan would be ceremony.
Use project-root-relative `research/` files when a task needs exploratory thinking, design discussion, decision capture, or investigation before planning or implementation.
If `research/` does not exist, do not create it automatically.
Ask first, unless the user explicitly asks to start research documentation or the repo already documents this workflow.

Research files should be Markdown files with YAML front matter:

```yaml
---
date: yyyy-mm-dd
updated: yyyy-mm-dd
---
```

Use `date` when the file is created.
Add or update `updated` only when modifying an existing research file.

A new research file should include:

```md
# Purpose

Succinctly describe the scope of the research.

# Research

Record findings, discussion, options, decisions, risks, and open questions.
```

Agents should only require and maintain the `# Purpose` and `# Research` sections.
If users add other headers, preserve them, but do not require or manage them unless explicitly asked.
When editing existing research files, preserve their current structure unless the user asks for normalization.
Research files should inform plan files, but a one-to-one research-to-plan pairing is not required.

# Plans

Use project-root-relative `plans/` files when implementation needs explicit alignment before code changes.
If `plans/` does not exist, do not create it automatically.
Ask first, unless the user explicitly asks to create a plan or the repo already documents this workflow.
If requested work is larger than a small single-file change and no plan exists, ask whether to create a plan before implementing.

Plan files should be Markdown files with YAML front matter:

```yaml
---
date: yyyy-mm-dd
updated: yyyy-mm-dd
---
```

Use `date` when the file is created.
Add or update `updated` only when modifying an existing plan file.

A new plan file should include:

```md
# Purpose

Succinctly describe the implementation scope and non-goals.

# Tasks

Track the work as an ordered checklist.
```

Plan tasks should be specific enough to execute and review.
Include file targets, expected changes, expected results, validation commands, assumptions, risks, and known out-of-scope work when relevant.

- Plans are detailed enough to execute without guessing
- Plans are narrowly scoped
- Task sections should follow a logical or chronological order
- Show the user, don't tell the user; tasks should name the file, change, expected result, and validation
- Tasks may contain code, and when they do, the code should be fully scoped so the user can review exactly what will be written before implementation
- Identify tasks that can be completed in parallel

Use unchecked boxes for planned work and check them off only after completion:

```md
- [ ] Make the change
- [ ] Run the validation
- [ ] Record any implementation notes
```

While creating or reviewing a plan, do not implement it unless the user explicitly asks.

If Ponytail is installed, use it during planning to avoid over-engineering and prefer the smallest correct implementation.

During implementation of an approved plan:

- Adhere strictly to the plan unless new information makes it wrong
- If the plan becomes wrong or unsafe, stop, ask, and record the issue in the plan
- Update task checkboxes and implementation notes as work completes
- Do not stop until all planned tasks are complete, blocked, or explicitly deferred
- Preserve unrelated worktree changes
- Run the smallest meaningful validation for the change
- Record implementation questions, blockers, deviations, and validation failures in the plan

If Roborev is installed, include a final commit task:

```md
## Commit

- [ ] Inspect status, diff, and recent commits
- [ ] Stage only files relevant to this plan, including the plan file
- [ ] Commit without pushing
- [ ] Do not push; the user will manually initiate Roborev review
```

If Roborev is not installed, do not include a commit task unless the user explicitly requests one.

# Working without a plan

When the user chooses to work without a plan:

- make the smallest useful change that satisfies the request
- run the smallest meaningful validation for non-trivial edits
- if validation is not run, say why
