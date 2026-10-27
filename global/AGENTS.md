# Tools

- If `rtk` is installed, it runs as an opencode hook and cannot be bypassed by agents
- If `roborev` is installed (`roborev version` exits 0), it handles PR review; agents must not push and should always include a commit task in plans

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
status: open
---
```

Use `date` when the file is created.
Add or update `updated` only when modifying an existing research file.
`updated` must not be later than today's date.
Add `status: open` when a research file is created.
The implementing agent must edit the front matter to update status to `in-progress` when a plan begins implementing the research; keep it if the plan stops partway.
Add `status: resolved` when a research file has been completed/resolved.
Set `status: abandoned` when the research is dropped without being resolved.

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
An early task in each plan must mark `status: in-progress` in the research file that informed it, if any.
Once a plan is implemented, agents must mark `status: resolved` in that research file.
Plan files should be Markdown files with YAML front matter:

```yaml
---
date: yyyy-mm-dd
status: open
---
```

Use `date` when the file is created.
Add or update `updated` only when modifying an existing plan file.
`updated` must not be later than today's date.
Add `status: open` when a plan file is created.
The implementing agent must edit the front matter to update status to `in-progress` when the first task starts; keep it if the plan stops partway.
Set `status: resolved` when the plan is fully implemented.
Set `status: superseded` when another plan replaces it, and link the replacing plan's path under `# Purpose`.
Set `status: abandoned` when the plan is dropped with no replacement, and give the reason under `# Purpose`.

A new plan file should include:

```md
# Purpose

Succinctly describe the implementation scope and non-goals.

# Tasks

Track the work as an ordered checklist.
```

## Task format

Each task edits exactly one file (or runs one command, see below) and must contain all of these fields:

- File: project-root-relative path
- Lines: line range in the file as it exists before any task runs, plus a unique anchor string for the target (line numbers drift as earlier tasks land)
- Change: one sentence stating the intent
- Before: the exact current text in a code block (omit for new files)
- After: the exact new text in a code block (omit for deletions); never use placeholders such as `...` or "similar to above"
- Expected result: observable outcome, such as a diff stat, output, or behavior
- Validate: runnable command and the output that means success

New file: show the complete content under After.
Deletion: show the removed text under Before and state why.
Non-code files: Before and After still apply.

Command-only task (no file edit, e.g. install a dependency, run a migration): replace File/Lines/Before/After with a single `Command` field giving the exact command to run; Change and Expected result still apply. If the command has no independent check, Validate may restate the command's own output as the success condition.

The generic checklist example and the Roborev `## Commit` checklist below are each a self-contained checklist, not a Task-format entry; they don't need their own File/Lines/Before/After/Validate.

Example:

````md
- [ ] Task 3: tighten error message
  - File: `src/load.py`
  - Lines: 41-43 (anchor: `raise ValueError("bad input")`)
  - Change: include the offending path in the error
  - Before:
    ```python
    raise ValueError("bad input")
    ```
  - After:
    ```python
    raise ValueError(f"bad input: {path}")
    ```
  - Expected result: error text ends with the file path
  - Validate: `uv run pytest tests/test_load.py -k bad_input` passes
````

Plan-level rules:

- Plans are detailed enough to execute without guessing and are narrowly scoped
- Order tasks logically or chronologically; order edits to the same file bottom-up, or re-anchor by string, so line numbers stay valid
- Mark parallel-safe tasks with `[parallel: A]` and dependent tasks with `[after: 3]`
- State assumptions, risks, and out-of-scope work once in `# Purpose`, not per task
- A task that cannot name its Before and After is not ready; add an investigation task first
- Record implementation notes and deviations under the task they affect

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

If Roborev is installed (detect: run `roborev version`; exit 0 means installed), include a final commit task:

```md
## Commit

- [ ] Inspect status, diff, and recent commits
- [ ] Stage only files relevant to this plan, including the plan file
- [ ] Commit without pushing
- [ ] Do not push; the user will manually initiate Roborev review
```

If Roborev is not installed (`roborev version` fails), do not include a commit task unless the user explicitly requests one.

# Working without a plan

When the user chooses to work without a plan:

- make the smallest useful change that satisfies the request
- run the smallest meaningful validation for non-trivial edits
- if validation is not run, say why
