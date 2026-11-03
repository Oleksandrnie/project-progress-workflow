# Skill: Project Progress Workflow

Author: [Oleksandr Niemchenko](https://github.com/Oleksandrnie) · License: [MIT](LICENSE)

A Codex skill for planning, tracking, validating, and completing substantial work in a Git repository.

## When to use it

Use `project-progress-workflow` for features, substantial bug fixes, refactors, migrations, infrastructure changes, and multi-file documentation work. It is not intended for read-only explanations or small isolated edits.

## What it does

The skill helps an agent establish task scope, inspect repository instructions and current Git state, divide work into stages, select suitable checks, and preserve a continuation record while work is underway. At the end it checks completion criteria, updates relevant project records, removes temporary state, and reports what was done.

The progress file is kept locally under `temporary/` and excluded from Git. It may be lost if the local worktree is deleted. A direct invocation authorizes focused local checkpoint commits after non-empty stages, unless a higher-priority instruction forbids them. When the skill is selected implicitly and no commit strategy was agreed, the agent asks before the first commit. It never pushes automatically and must preserve unrelated user changes.

## Requirements

Use it in a Git repository with an agent that can read and write project files and run Git commands. The skill has no additional package dependencies or external service requirements.

## Install in Codex

Copy the complete `project-progress-workflow` directory into the Codex skills directory:

```text
~/.codex/skills/project-progress-workflow/
```

Then invoke it with a request such as:

```text
Use $project-progress-workflow to refactor this module in reviewable stages.
```

You can override its defaults in the same request, for example: “Do not create commits.”

## Package contents

- `SKILL.md` contains the workflow and skill metadata.
- `agents/openai.yaml` provides Codex interface metadata.
- `references/progress-template.md` is the local progress-file template.
- `evals/evals.json` contains four scenarios for reviewing expected behavior. They are evaluation prompts, not an automated test runner.
- `LICENSE` contains the MIT license.

The skill has been smoke-tested on a small refactor in a temporary Git repository. The four included evaluation scenarios have not been run as a full evaluation suite. Agent behavior depends on the host agent and its runtime.

## Feedback

Open an issue or suggest an improvement via [the author's GitHub profile](https://github.com/Oleksandrnie).
