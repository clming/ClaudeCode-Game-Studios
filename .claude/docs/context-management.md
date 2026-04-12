# Context Management

Context is the most critical resource in a Claude Code session. Manage it actively.

## File-Backed State (Primary Strategy)

**The file is the memory, not the conversation.** Conversations are ephemeral and
will be compacted or lost. Files on disk persist across compactions and session crashes.

### Session State File

Maintain `production/session-state/active.md` as a living checkpoint. Update it
after each significant milestone:

- Design section approved and written to file
- Architecture decision made
- Implementation milestone reached
- Test results obtained

The state file should contain: current task, progress checklist, key decisions
made, files being worked on, and open questions.

Follow the detailed state-file rules in `.claude/docs/session-state-rules.md`.

### Status Line Block (Production+ only)

When the project is in Production, Polish, or Release stage, include a structured
status block in `active.md` that the status line script can parse:

```markdown
<!-- STATUS -->
Epic: Combat System
Feature: Melee Combat
Task: Implement hitbox detection
<!-- /STATUS -->
```

- All three fields (Epic, Feature, Task) are optional — include only what applies
- Update this block when switching focus areas
- The status line displays it as a breadcrumb: `Combat System > Melee Combat > Hitboxes`
- Remove or empty the block when no active work focus exists

After any disruption (compaction, crash, `/clear`), read the state file first.

### Incremental File Writing

When creating multi-section documents (design docs, architecture docs, lore entries):

1. Create the file immediately with a skeleton (all section headers, empty bodies)
2. Discuss and draft one section at a time in conversation
3. Write each section to the file as soon as it's approved
4. Update the session state file after each section
5. After writing a section, previous discussion about that section can be safely
   compacted — the decisions are in the file

This keeps the context window holding only the *current* section's discussion
(~3-5k tokens) instead of the entire document's conversation history (~30-50k tokens).

## Proactive Compaction

- **Compact proactively** at ~60-70% context usage, not reactively at the limit
- **Use `/clear`** between unrelated tasks, or after 2+ failed correction attempts
- **Natural compaction points:** after writing a section to file, after committing,
  after completing a task, before starting a new topic
- **Focused compaction:** `/compact Focus on [current task] — sections 1-3 are
  written to file, working on section 4`

## Context Budgets by Task Type

- Light (read/review): ~3k tokens startup
- Medium (implement feature): ~8k tokens
- Heavy (multi-system refactor): ~15k tokens

## Subagent Delegation

Use subagents for research and exploration to keep the main session clean.
Subagents run in their own context window and return only summaries:

- **Use subagents** when investigating across multiple files, exploring unfamiliar code,
  or doing research that would consume >5k tokens of file reads
- **Use direct reads** when you know exactly which 1-2 files to check
- Subagents do not inherit conversation history — provide full context in the prompt

## Compaction Instructions

When context is compacted, preserve the following in the summary:

- Reference to `production/session-state/active.md` (read it to recover state)
- List of files modified in this session and their purpose
- Any architectural decisions made and their rationale
- Active sprint tasks and their current status
- Agent invocations and their outcomes (success/failure/blocked)
- Test results (pass/fail counts, specific failures)
- Unresolved blockers or questions awaiting user input
- The current task and what step we are on
- Which sections of the current document are written to file vs. still in progress

**After compaction:** Read `production/session-state/active.md` and any files being
actively worked on to recover full context. The files contain the decisions; the
conversation history is secondary.

## Recovery After Session Crash

If a session dies ("prompt too long") or you start a new session to continue work:

1. The `session-start.sh` hook will detect and preview `active.md` automatically
2. Read the full state file for context
3. Read the partially-completed file(s) listed in the state
4. Continue from the next incomplete section or task

## Session Boundary Rules

Agents should proactively suggest starting a new conversation when doing so will
improve clarity, speed, or accuracy.

### When to Suggest a New Conversation

Suggest a fresh conversation when any of the following is true:

1. **Task Completed** -- The primary goal of the current conversation is done.
   Summarize what was completed, then suggest a new conversation for the next task.
2. **Topic Drift** -- The user shifts to a substantially different task
   (for example, moving from bug fixing to sprint planning or from GDD writing
   to release work).
3. **Role Switch** -- The task now clearly belongs to a different specialist
   role or team flow (for example, switching from `gameplay-programmer` work
   to `producer` planning or `qa-lead` verification).
4. **Context Overload** -- The conversation has exceeded roughly 30 back-and-forth
   rounds, or the session is becoming slow, repetitive, or context-fragile.

### How to Handoff

When suggesting a new conversation:

1. Summarize the current task's outcome or current stopping point
2. State why a fresh conversation would help
3. Provide a concrete, copy-paste-ready opening prompt
4. Include the role, next task, and the minimum prior context needed

Use this format:

```text
[Current task summary]
Start a new conversation for the next task with:
"[Suggested opening prompt with role, task, and prior context summary]"
```

### Handoff Quality Standard

A good handoff prompt should:

- Name the role or workflow to use next
- State the exact next objective
- Include only the critical prior context
- Be immediately usable without reconstruction work from the user

If a task is still actively in progress and the current conversation remains
focused, do not force a new conversation. This is a proactive suggestion rule,
not a hard stop rule.
