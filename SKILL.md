---
name: project-progress-workflow
description: Plan, track, validate, checkpoint, and close non-trivial work in a Git repository. Use for features, substantial bug fixes, refactors, migrations, infrastructure or documentation restructuring, multi-file changes, tasks with a specification or Definition of Done, staged implementation, progress tracking, handoff requirements, or one-commit-per-stage expectations. An explicit use of this workflow authorizes a focused local checkpoint commit after each non-empty stage without committing temporary state; update affected permanent project records and remove the progress file only after genuine completion. Do not use for read-only investigation, explanations, or trivial isolated edits that do not benefit from staged execution.
license: MIT
compatibility: Requires a Git repository and an agent able to read and write files and run Git commands.
metadata:
  version: "0.1.0"
  author: Oleksandr Niemchenko
  github: https://github.com/Oleksandrnie
---

# Skill: Project Progress Workflow

Control a repository task from initial analysis through recoverable staged implementation and verified cleanup. Treat the progress file as local-only model execution state, never as permanent project documentation or Git history. The progress file may be lost if its local worktree is deleted; this is an accepted tradeoff for keeping temporary workflow data out of the repository.

## Resolve authority and Git authorization

Apply instructions in this order:

1. Direct user instructions.
2. Task-specific specification and acceptance criteria.
3. Repository instruction files named `AGENT.md` or `AGENTS.md`, matched case-insensitively; the nearest applicable file takes precedence within its scope.
4. This skill.
5. Current permanent project documentation.
6. Verified implementation patterns.

Record material conflicts and their resolution in the progress file. Never let this skill override a higher-priority instruction.

Model routing is defined by the nearest applicable repository `AGENTS.md`; treat it as the canonical policy and do not copy model assignments into this skill, project plans, or progress files. A progress record may capture the model and effort actually selected for a stage as execution metadata, but that record is not a routing rule. When delegating, pass the selected model and effort explicitly if the runtime supports them; the instruction file guides the choice but does not configure the runtime. Direct user instructions still override the repository default for the current task.

An explicit invocation of this workflow, or a direct request for staged or checkpoint commits, authorizes a focused local checkpoint commit after each non-empty stage. If the workflow is selected implicitly for a generic multi-file task and the user has not given a commit strategy, ask once before the first commit. Before committing, verify that no direct user instruction, task specification, or repository instruction forbids commits; those higher-priority instructions override this default. A checkpoint may contain a known failing test or other validation error: preserve the state for rollback, record the exact failure, and do not mark the stage `completed` until the issue is fixed. Never create empty commits, commit research-only stages, include unrelated changes, or push automatically.

Never discard, overwrite, stage, or commit unrelated user changes. Never force-push, rewrite shared history, or create/switch branches without authorization.

## Analyze before source edits

Before changing source, configuration, migrations, or permanent documentation:

1. From the repository root, search the entire project for files whose basename case-insensitively matches `AGENT.md` or `AGENTS.md`. Read every discovered file before beginning analysis or implementation. Apply each file within its declared scope; when scope is not stated, treat it as repository-wide. Before editing within a nested area, re-check which discovered instruction files apply there.
2. Read the task specification.
3. Inspect the minimum relevant implementation, tests, and documentation.
4. Verify documentation claims against current code and configuration.
5. Capture the current branch, working-tree state, staged/untracked files, relevant behavior, tests, and known failures.
6. Separate confirmed facts, assumptions, unresolved questions, and future ideas.
7. Lock scope: goal, measurable result, in-scope work, out-of-scope work, affected areas, dependencies, compatibility needs, risks, and rollback concerns.
8. Divide the work into the smallest coherent, reviewable stages.

Do not broaden the task for adjacent improvements. Record useful non-blocking discoveries as deferred work and update a backlog only when the repository has one and the finding is actionable.

## Choose validation scope

Before running baseline or stage validation, decide whether checks should cover the affected area or the entire project:

- Use focused validation by default for a small, local change in a simple project.
- Ask the user one concise question when full-project validation would materially increase time and both scopes are reasonable: whether to check the whole project or only the affected area.
- Use full-project validation when the user requests it or when the change affects shared configuration, public interfaces, migrations, build/tooling, multiple modules, or cross-cutting behavior.
- If focused checks expose a cross-area regression, escalate to full-project validation and record the reason.
- Record the selected scope, checks run, skipped checks, and escalation decision in the progress file.

## Create exactly one progress file

Before the first tracked implementation change, create:

`temporary/<short-kebab-task-name>-progress.md`

Create `temporary/` when it does not exist. Before creating the file, verify that `temporary/` is ignored by Git. If it is not ignored, add `temporary/` to the repository-local `.git/info/exclude`; do not modify the project's `.gitignore` solely for this workflow. If `temporary/` already contains tracked files, report the conflict and do not untrack them automatically. The directory name deliberately marks its contents as local temporary execution state; do not add `.gitkeep` solely for this workflow. If repository instruction files explicitly define another canonical progress location, use that location and record the reason. Never maintain two active progress files for one task.

Build the file from [progress-template.md](references/progress-template.md). Keep it concise and truthful. Do not store secrets, personal data, raw logs, or duplicate the full specification.

The progress file and every file below `temporary/` are local-only. Never add them to the Git index, never include them in a checkpoint commit, never update permanent documentation to reference them, and never use `git add .` or `git add -A` while they are present. Stage only explicit permanent files owned by the current stage.

If continuing existing work, locate the single active task progress file, reconcile it with Git history and the working tree, and update stale state before editing. Do not create a replacement merely because the existing plan is incomplete.

## Plan stages

Give every stage:

- one clear goal and expected result;
- concrete actions and affected areas;
- dependencies on earlier stages;
- targeted validation and a measurable completion criterion;
- a proposed focused commit message when the stage changes repository files.

Use one commit per stage, not one stage per arbitrary file. Do not create empty commits or commits for research-only stages. A final cleanup stage may delete the local-only progress file and update permanent records, but never stages or commits that file or its deletion.

## Execute and checkpoint each stage

For each stage:

1. Mark it `in_progress` and record the next exact action.
2. Make the smallest coherent change inside the scope lock.
3. Add or update tests for changed behavior where applicable.
4. Run the planned targeted checks, including relevant failure paths and regressions.
5. Compare failures with the recorded baseline; never relabel a new regression as pre-existing without evidence.
6. Inspect the complete diff and working tree for accidental or unrelated changes.
7. Update decisions, changed files, validation results, risks, and continuation state.
8. Mark the stage `completed` only when its criterion is satisfied.
9. If the stage produced any stage-owned file changes, including new untracked files, stage only those files and create its focused checkpoint commit, even when validation found a failure. Use a message that identifies the stage and, when applicable, marks the checkpoint as containing a known failure. Record the actual commit hash, message, validation result, and rollback value in the next progress update or subsequent checkpoint. Do not mark a failed stage `completed` merely because its checkpoint was created.

Use only these stage statuses: `pending`, `in_progress`, `blocked`, `completed`, `deferred`, `cancelled`.

Report validation precisely: commands run, results, skipped checks, remaining uncertainty, and pre-existing failures. Static checks alone do not prove runtime behavior.

## Handle interruption

When stopping before completion, keep the progress file. Update it with the current stage, completed work, changed files, validations, known failures, last safe checkpoint, repository state, next exact action, and next command or file to inspect. Do not mark unfinished work complete.

## Close the task

Begin final cleanup only after every approved stage satisfies its completion criterion.

1. Re-read the specification and check every requirement and Definition of Done item.
2. Run the complete proportional validation set and inspect the final diff and working tree.
3. Update only permanent documentation affected by the implemented result.
4. Discover the repository's existing permanent completion record, such as a project index, task index, changelog, release notes, or completed-work ledger. Update only the canonical record required by repository instructions or established practice. Do not invent a new index when none exists.
5. Move deferred actionable work to the existing backlog when appropriate.
6. Remove task-owned temporary artifacts. The progress file and all task-owned files under `temporary/` must remain uncommitted throughout the task.
7. Delete the progress file only when implementation and validation are complete, permanent records are current, deferred work is captured, no continuation state is needed, and final repository state is understood.
8. If this workflow created `temporary/` and it is empty after deleting the progress file, remove the directory as well. Never remove unrelated files from it. Leave the local ignore rule in place if it protects future temporary workflow state.
9. Include only permanent-record updates in the final cleanup checkpoint when they belong to the task. Record the local progress-file deletion in the final report; the default checkpoint-commit rule still applies unless a higher-priority instruction forbids commits.

Keep the progress file when work is incomplete, blocked, partially validated, awaiting approval, or dependent on unavailable external work.

## Report the outcome

Return a concise report covering:

- completed stages and user-visible result;
- important decisions;
- exact validation performed and skipped;
- branch, checkpoint commits actually created, and working-tree state;
- selected route and effective model/effort only when runtime metadata makes them available; otherwise state that they were unavailable;
- permanent documents or index entries updated;
- confirmation that the progress file was deleted, or the exact reason it remains;
- risks, pre-existing failures, deferred work, and next action if incomplete.

Never claim a commit, push, branch, test, or external update that was not verified.
