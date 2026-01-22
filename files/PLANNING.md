# Planning and Execution

## Quick Start

1. Create project folder: `.claude/projects/{project-name}/`
2. Create plan file: `{PROJECT}_PLAN.md`
3. Create progress tracker: `{PROJECT}_PROGRESS.md`
4. Review plan with user
5. Execute with parallel agents

---

## Project Folder Structure

```
.claude/projects/{project-name}/
├── {PROJECT}_PLAN.md
├── {PROJECT}_PROGRESS.md
└── LEARNINGS.md
```

---

## Plan Template

Write to `.claude/projects/{project-name}/{PROJECT}_PLAN.md`:

```markdown
# {Project Name} Plan

## Overview
[1-2 sentences describing what this plan accomplishes]

## Goals
- [Primary goal 1]
- [Primary goal 2]

## Non-Goals
- [Out of scope item 1]

## Phases

### Phase 1: [Name] (parallel|sequential)
**Dependencies**: None
**Agents**: [Number]

#### Task 1.1: [Task Name]
- **Files**: [List specific files this task owns]
- **Steps**:
  1. [Explicit step]
  2. [Explicit step]
- **Success Criteria**: [Verifiable outcome]

#### Task 1.2: [Task Name]
[Same structure]

### Phase 2: [Name] (parallel|sequential)
**Dependencies**: Phase 1
[Same structure]

## Success Criteria
- [ ] [Verifiable outcome 1]
- [ ] [Verifiable outcome 2]
```

---

## Progress Tracker Template

Write to `.claude/projects/{project-name}/{PROJECT}_PROGRESS.md`:

```markdown
# {Project Name} Progress

## Status: In Progress

## Phase 1: [Name]

| Task | Status | Agent | Started | Completed |
|------|--------|-------|---------|-----------|
| Task 1.1 | Pending | - | - | - |
| Task 1.2 | Pending | - | - | - |

## Blockers
[None currently]
```

Status values: `Pending`, `In Progress`, `Blocked`, `Completed`

---

## Spawning Execution Agents

Agents do NOT inherit conversation context. Include everything they need directly in the prompt.

**IMPORTANT**: Direct all spawned agents to follow `.claude/docs/setup/AGENT_WORKFLOW.md` for progress tracking and learnings.

### Agent Prompt Template

```markdown
## Task: [Task Name]

You are execution-agent-{N}.

### Your Assignment
- **Project**: `.claude/projects/{project-name}/`
- **Task**: [Task name from plan]
- **Files you own**: [List files - ONLY modify these]

### Steps
1. [Explicit step from plan]
2. [Explicit step from plan]

### Success Criteria
[Verifiable outcome]

### Workflow
Follow `.claude/docs/setup/AGENT_WORKFLOW.md` for detailed instructions.

1. Read the plan at `.claude/projects/{project-name}/{PROJECT}_PLAN.md`
2. Mark your task "In Progress" in `{PROJECT}_PROGRESS.md` with timestamp
3. Complete your assigned work
4. Add any learnings to `LEARNINGS.md`
5. Mark your task "Completed" in progress tracker with timestamp
```

---

## Parallel Execution Rules

### File Ownership
Each task MUST own specific files. No two agents modify the same file.

```markdown
#### Task 2.1: Implement Auth
- **Files**: src/auth/login.py, src/auth/session.py, tests/test_auth.py

#### Task 2.2: Implement Routes
- **Files**: src/routes/api.py, src/routes/handlers.py, tests/test_routes.py
```

### When to Parallelize

| Scenario | Parallelize? |
|----------|-------------|
| Tasks touch different files | Yes |
| Task B needs Task A's output | No - sequence |
| Both tasks modify same file | No - sequence |
| "All UI tasks" grouping | Yes - group by independence, not type |

### The Golden Rule

**Group tasks by independence, not by type.**

```
GOOD:
├── Phase 1 (parallel) - 3 agents
│   ├── Task: Set up auth module
│   ├── Task: Set up routing module
│   └── Task: Set up config files
└── Phase 2 (sequential) - 1 agent
    └── Task: Integration testing

BAD:
Phase 1 → Phase 2 → Phase 3 → Phase 4 (all sequential, unnecessary)
```

---

## Plan Quality Checklist

Before executing, verify:

- [ ] Every task has explicit file ownership
- [ ] No two tasks modify the same file
- [ ] Phase dependencies are stated
- [ ] Each task has verifiable success criteria
- [ ] Steps are explicit (agent has no prior context)
- [ ] Progress tracker exists alongside plan
