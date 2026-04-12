# Active Session State

## Current Task

- Primary task: Finalize a production-ready `active.md` standard and working state file for this collaboration template
- Current stage: Studio Governance / Session-State Design
- Active role: technical-director / lead-programmer

## Progress

- [x] Clarified the difference between conversation memory and file-backed state
- [x] Documented how session recovery works across compaction, restart, and interruption
- [x] Defined a concise `active.md` maintenance standard
- [x] Created the initial session-state file at `production/session-state/active.md`
- [ ] Finalize this file as the default production-ready working state
- [ ] Decide whether to propagate the English `active.md` pattern into additional onboarding docs

## Confirmed Decisions

- File-backed state is the primary continuity mechanism; conversation history is secondary
- `production/session-state/active.md` must stay concise, high-signal, and focused on one primary task
- Task switches must update `Current Task`, `Progress`, `Active Files`, and `Next Action`
- `active.md` should be maintained in English for consistency and easier cross-agent handoff

## Active Files

- `production/session-state/active.md`
- `docs/active.md维护规范.md`
- `docs/AI协作中的内存、上下文与共享信息机制.md`
- `docs/文档分层说明.md`

## Open Questions

- Should the English `active.md` pattern remain a single live file only, or also be added as a reusable example in `docs/`
- Should future state entries always use one active role, or allow paired ownership for cross-domain planning tasks

## Next Action

- Next step: Keep this file as the live English session state baseline and update it only at real task-transition checkpoints

<!-- STATUS -->
Epic: Studio Governance
Feature: Session State Management
Task: Production-ready active.md baseline
<!-- /STATUS -->
