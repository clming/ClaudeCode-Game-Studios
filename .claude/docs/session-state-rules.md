# active.md Maintenance Rules

## 1. Purpose

`production/session-state/active.md` is the project's live state bridge between:

- transient conversation memory
- recoverable task context
- persistent project files

Its job is not to describe everything that happened. Its job is to let the next session, the next agent, or a restarted window continue work quickly and correctly.

The quality bar is simple:

- high accuracy
- low verbosity
- fast recovery
- low token cost

---

## 2. State Model

To use `active.md` correctly, keep these three layers separate.

## 2.1 Memory

Memory means the temporary information held inside the current conversation window.

Properties:

- fast to use
- easy to lose
- degraded by compaction, restarts, topic drift, and long chats

Rule:

- never rely on conversation memory as the system of record

---

## 2.2 Context

Context means the distilled facts needed to continue the current task safely.

Examples:

- current objective
- current progress
- confirmed decisions
- active files
- blockers
- next action

Rule:

- context must be distilled into `production/session-state/active.md`

---

## 2.3 Shared Project Knowledge

Shared project knowledge lives in persistent files such as:

- `CLAUDE.md`
- `.claude/agents/*`
- `.claude/rules/*`
- `.claude/skills/*`
- `.claude/docs/*`
- `design/*`
- `docs/*`
- `src/*`
- `tests/*`
- `production/*`

Rule:

- `active.md` should reference these files, not duplicate them

---

## 3. Core Rules

## 3.1 One Primary Task

`active.md` must track one primary task at a time.

If work changes substantially, update the file to reflect the new primary task instead of keeping parallel task histories.

---

## 3.2 Record State, Not Discussion

`active.md` must contain:

- decisions
- progress checkpoints
- blockers
- next action

It must not contain:

- long explanations
- debate history
- speculative ideas not yet approved
- copied document sections
- copied code

---

## 3.3 Keep It Small

Target size:

- normal use: 20 to 60 lines
- complex use: hard limit 80 lines

If the file grows beyond that, compress it into conclusions and move detail back into the real project files.

---

## 3.4 Reference Files, Do Not Mirror Them

The `Active Files` section should list only the files that must be read first when recovering work.

Rule:

- list the smallest useful set
- usually 3 to 8 files
- never dump a large directory inventory

---

## 3.5 Decisions Must Be Final

The `Confirmed Decisions` section may contain only decisions that are already approved.

If something is still uncertain, it belongs in `Open Questions`, not in `Confirmed Decisions`.

---

## 3.6 Next Action Must Be Immediate

`Next Action` must answer this question:

> If the session stops now, what should happen first when work resumes?

Rule:

- write one clear action
- use an action verb
- avoid vague phrases like "continue work"

---

## 4. Required Structure

`production/session-state/active.md` should always use this structure:

```md
# Active Session State

## Current Task

- Primary task:
- Current stage:
- Active role:

## Progress

- [ ] 

## Confirmed Decisions

- None yet

## Active Files

- ``

## Open Questions

- None

## Next Action

- Next step:

<!-- STATUS -->
Epic:
Feature:
Task:
<!-- /STATUS -->
```

---

## 5. Field Semantics

## 5.1 `Current Task`

This is the task anchor.

It must define:

- what is being done now
- which stage the work is in
- which role currently owns the work

Rule:

- use one task only
- do not mix unrelated tasks

---

## 5.2 `Progress`

This is the recovery checklist.

Rule:

- keep 3 to 7 items where possible
- write action-oriented checkpoints
- remove stale items when the task changes

---

## 5.3 `Confirmed Decisions`

This is the anti-regression section.

Its purpose is to stop future sessions from re-arguing already-approved decisions.

Rule:

- every line must be stable and approved
- if not approved, move it to `Open Questions`

---

## 5.4 `Active Files`

This is the recovery reading order.

Rule:

- include only files that the next session must read first
- prefer exact files over folders
- keep the list short

---

## 5.5 `Open Questions`

This is the blocker section.

Rule:

- only include real blockers or pending user decisions
- if there are no blockers, write `None`

---

## 5.6 `Next Action`

This is the restart trigger.

Rule:

- write one immediate next step
- keep it concrete and executable

---

## 6. Task Transition Rules

This is the most important part for efficient project design and smooth handoff.

## 6.1 What Counts as a Task Transition

Any of the following means the task has changed:

1. the primary objective changes
2. the active role changes
3. the main deliverable changes
4. the active file set changes substantially

Examples:

- from studio design to gameplay design
- from planning to implementation
- from `lead-programmer` work to `producer` work

---

## 6.2 What Must Be Updated at Transition Time

At minimum, update:

1. `Current Task`
2. `Progress`
3. `Active Files`
4. `Next Action`

Update these too if needed:

- `Confirmed Decisions`
- `Open Questions`
- `STATUS`

---

## 6.3 What Must Be Removed at Transition Time

When a task changes, remove:

- stale progress items
- stale active files
- temporary thinking from the previous task

Rule:

- `active.md` must not carry two worlds at once

---

## 7. Update Timing

Do not update on every message. Update at high-recovery points:

1. after a key decision is approved
2. after a document section is written
3. after a meaningful implementation checkpoint
4. before compaction
5. before closing the window
6. before switching topics
7. before starting a new conversation

---

## 8. Token Efficiency Rules

The purpose of `active.md` is partly to preserve quality without wasting tokens.

To achieve that:

1. store conclusions, not full reasoning
2. reference files, do not restate them
3. keep one primary task
4. delete stale items aggressively
5. keep blockers short and explicit
6. use `Confirmed Decisions` to prevent repeated re-analysis

If a detail requires more than one or two lines, it probably belongs in a real project file instead of `active.md`.

---

## 9. Recovery Protocol

After compaction, restart, crash, or handoff:

1. read `production/session-state/active.md`
2. read the files listed in `Active Files`
3. continue with `Next Action`

This order matters.

Do not try to reconstruct the whole task from chat history first.

---

## 10. Quality Test

A good `active.md` should pass all five checks below:

1. readable in under 3 minutes
2. one obvious primary task
3. one obvious next step
4. no stale task residue
5. enough context to resume without digging through old chat

If it fails any of these checks, compress and rewrite it.
