# Agent Workflow

Instructions for execution agents working on project tasks.

---

## When You Start

1. Read the plan at `.claude/projects/{project}/`
2. Mark your task **"In Progress"** in the progress tracker with timestamp
3. Complete your assigned work
4. Add learnings to `LEARNINGS.md` if you discover something useful
5. Mark your task **"Completed"** in progress tracker with timestamp

---

## Updating Progress

Edit the progress tracker at `.claude/projects/{project}/{PROJECT}_PROGRESS.md`:

| Status | When to Use | Action |
|--------|-------------|--------|
| In Progress | Starting task | Add your agent ID and timestamp |
| Blocked | Waiting on something | Add reason in Blockers section |
| Completed | Task done | Add completion timestamp |

**Example update:**

```markdown
| Task 1.1 | In Progress | agent-1 | 2024-01-15 10:30 | - |
```

When done:

```markdown
| Task 1.1 | Completed | agent-1 | 2024-01-15 10:30 | 2024-01-15 11:45 |
```

---

## Adding Learnings

Add discoveries to `.claude/projects/{project}/LEARNINGS.md` (newest first):

```markdown
## YYYY-MM-DD HH:MM - [Tag] Title

**Context**: What were you trying to do?
**Discovery**: What did you learn?
**Why it matters**: How does this help others?
**Related**: `path/to/file.py:123`
```

**Tags**: `[Bug]`, `[Pattern]`, `[Gotcha]`, `[API]`, `[Performance]`, `[Testing]`

---

## Rules

**Do:**
- Update progress tracker immediately when starting and finishing
- Stay within your assigned file scope
- Add learnings as soon as you discover them
- Include timestamps in all updates

**Do not:**
- Modify files outside your assigned scope
- Leave stale "In Progress" markers
- Skip progress tracker updates
