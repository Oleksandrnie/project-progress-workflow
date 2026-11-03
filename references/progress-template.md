# Progress file template

Use this structure for the single active task file. Remove sections that are genuinely irrelevant, but retain scope, stages, validation, checkpoints, continuation state, and final checklist.

```markdown
# <Task Name> Progress

## Task

- Goal:
- Problem:
- Expected result:
- Source specification:
- Branch:
- Selected model and effort for this stage (execution metadata; routing policy is in applicable `AGENTS.md`):
- Effective executor metadata: verified value / unavailable
- Started:
- Current status:
- Current stage:

## Instructions and precedence

- Direct user requirements:
- Task-specific requirements:
- Discovered AGENT.md/AGENTS.md files and applicable rules:
- Conflicts and resolutions:
- Git authorization:
- Local-only progress state: temporary/ excluded from Git

## Scope lock

### In scope

- ...

### Out of scope

- ...

### Affected areas and expected files

- ...

### Dependencies and compatibility

- ...

### Risks and rollback concerns

- ...

## Baseline

- Current behavior:
- Existing tests:
- Validation commands:
- Validation scope: focused / full-project
- Scope decision and reason:
- Pre-existing failures:
- Branch and working-tree state:
- Unrelated user changes to preserve:
- Temporary files intentionally excluded from commits:

## Architecture

- Current flow:
- Target flow:
- Contracts and boundaries:
- Data and ownership:

## Implementation stages

### Stage <N> — <Name>

Repeat this block for each coherent stage. Use as many stages as the task needs; do not preserve a fixed stage count.

- Goal:
- Status: pending
- Actions:
- Expected result:
- Validation:
- Completion criterion:
- Affected files:
- Dependencies:
- Proposed checkpoint commit:

## Decisions

### <Decision title>

- Context:
- Decision:
- Reason:
- Alternatives:
- Consequences:

## Progress log

### <Date/time or checkpoint>

- Stage:
- Completed:
- Changed:
- Validation:
- Issues:
- Next action:

## Changed files

### Added

- ...

### Modified

- ...

### Deleted

- ...

## Validation results

- Static checks:
- Unit/contract tests:
- Integration/smoke tests:
- Manual/regression checks:
- Skipped checks and reasons:
- Remaining uncertainty:

## Regressions and deferred work

- New regressions:
- Pre-existing failures:
- Resolved issues:
- Unresolved blockers:
- Deferred/technical debt:

## Documentation and permanent records

- Architecture/testing/setup docs:
- Module knowledge docs:
- Changelog/release notes:
- Backlog:
- Canonical project/task index:

## Checkpoints

### Stage <N>

- Commit hash:
- Message:
- Files:
- Validation:
- Known failures:
- Rollback value:
- Progress file status: local-only, not committed
- Notes:

## Continuation state

- Current stage:
- Last completed action:
- Current repository state:
- Known risks:
- Next exact action:
- Next command or file:

## Final checklist

- [ ] Every approved stage is completed.
- [ ] Every acceptance criterion and Definition of Done item is checked.
- [ ] Required validation passed or limitations are explicitly accepted.
- [ ] Regressions were compared with baseline.
- [ ] Only affected permanent documentation and records were updated.
- [ ] Deferred work was captured without expanding scope.
- [ ] No unrelated changes or task-owned temporary artifacts remain.
- [ ] Final diff and working-tree state were inspected.
- [ ] Progress file is ready to be deleted.
```
