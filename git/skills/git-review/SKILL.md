---
name: git-review
description: >
  Diagnose a codebase using git history before reading any code. Use when
  inheriting a new project, onboarding to a new team, or assessing codebase
  health and risk. Covers five commands: (1) Code churn hotspots to identify
  high-risk files, (2) Bus factor analysis via commit authorship, (3) Bug
  cluster detection, (4) Project velocity trends, (5) Firefighting frequency.
  Append -- . to commands and run from app/ or src/ (not repo root) to scope
  results to that subdirectory and filter lockfiles/generated code.
  Always cross-reference churn against bug hotspots -- files on both are
  highest risk.
---

# Git Review: Codebase Diagnostic Commands

Run these five commands before reading any code. Present output as a summary
report with interpretation.

## Commands

### 1. Code Churn Hotspots

```bash
git log --format=format: --name-only --since="1 year ago" -- . | sed '/^$/d' | sort | uniq -c | sort -nr | head -20
```

Top 20 most-changed files in the last year.

### 2. Bus Factor

```bash
git shortlog -sn --no-merges -- .
git shortlog -sn --no-merges --since="6 months ago" -- .
```

Full history vs. recent (6-month) authorship. Flag if one person = 60%+ commits.

### 3. Bug Clusters

```bash
git log -i -P --grep='\bfix\b|\bbug\b|\bbroken\b' --name-only --format='' -- . | sed '/^$/d' | sort | uniq -c | sort -nr | head -20
```

Files most often associated with bug-related commits. Uses `-P` (Perl regex) for portable `\b` word boundaries; if PCRE is unavailable, fall back to `-E --grep='fix|bug|broken'` and manually filter false positives (e.g., "prefix", "suffix").

### 4. Project Velocity

```bash
git log --format='%ad' --date=format:'%Y-%m' -- . | sort | uniq -c
```

Monthly commit counts for the full history.

### 5. Firefighting Frequency

```bash
git log --oneline --since="1 year ago" -- . | grep -iE 'revert|hotfix|emergency|rollback'
```

Revert/hotfix/emergency commit count.

---

## Agent Output Format

After running these commands, present the findings as a structured report:

### Codebase Health Summary
- **Top churn file:** [filename] ([N] changes) — [flag if notable]
- **Bus factor risk:** [N]% from top contributor [name] — [flag if >60%]
- **Bug hotspots:** [list top 3 files appearing on churn AND bug lists]
- **Velocity trend:** [accelerating/stable/declining] — [key observation]
- **Firefighting frequency:** [N] incidents in last year — [flag if high]

### Flags to Raise
- Files that appear on BOTH churn and bug lists = highest risk, read these first
- Single contributor >60% commits = bus factor risk
- Velocity declining 6+ months = team momentum loss
- Frequent reverts/hotfixes (>1/month) = deploy fear signal

### Recommended Reading Order
1. [Highest risk file from churn+bug intersection]
2. [Second highest risk file]
3. ...

---

## Notes
- Run commands from `app/` or `src/` with `-- .` appended to scope to that directory
- Add `--no-merges` to exclude merge commits for cleaner authorship
- Bug cluster detection depends on descriptive commit messages
- Squash-merge workflows compress authorship to merger