---
name: nocobase-pr-workflow
description: Create a NocoBase task branch, commit current fix, push the remote branch, and prepare a pull request with the required template. Use when the user provides a taskid and asks to create a branch, commit, push, or open a PR for NocoBase work.
---

# NocoBase PR Workflow

Use this skill after a NocoBase fix is implemented and the user wants the branch, commit, push, and PR flow handled.

## Inputs

- `taskid`; if the user sends a plain number, treat it as the task ID.
- Current base branch must be `main`, `next`, or `develop`. Do not invent other base branches.
- PR type: bug fix, improvement, new feature, or other. Infer from context when obvious; ask only if risky.

## Workflow

1. Confirm the repo state with `git status --short` and current branch with `git branch --show-current`.
2. Ensure the current branch is `main`, `next`, or `develop`. If not, stop and ask which allowed base branch to use.
3. Create or switch to `task-{taskid}` from the current base branch.
4. Review the diff and generate a concise conventional commit from the actual changes.
5. Stage only relevant files, commit, and push with `git push -u origin task-{taskid}`.
6. Draft a PR using the template below. Populate motivation, description, related issue/task, changelog, docs/checklists, and testing notes from the diff and conversation.
7. Before creating or publishing the PR, show the final PR title and body and ask the user to confirm.
8. After confirmation, create the PR with the repo's available tool, preferably `gh pr create`, then output the PR link.

## Branch Rules

- Branch name: `task-{taskid}`.
- For bug fixes or non-feature modifications, base should normally be `main`.
- For new features or API modifications, base should normally be `next`.
- `develop` is allowed only when the repo/user is already on it or explicitly requests it.
- Never rebase, reset, or discard local changes unless the user explicitly asks.

## Commit Rules

- Base the commit message on the actual diff.
- Prefer conventional commits, for example `fix: ...`, `feat: ...`, `chore: ...`.
- Keep one task in one commit unless the existing diff clearly contains unrelated work; then ask before staging.

## PR Template

```md
<!--
First of all, thank you for your contribution!
For bug fixes or other non-feature modifications, please base your branch on the main branch.
For new features or API modifications, please make sure your branch is based on the next branch.
Thank you!
-->

### This is a ...
- [ ] New feature
- [ ] Improvement
- [ ] Bug fix
- [ ] Others

### Motivation
<!-- Please explain the reason of the changes made in this PR. -->

### Description
<!--
Please describe the key changes made in this PR clearly and concisely,
mention any potential risks,
and provide some testing suggestions.
-->

### Related issues

### Showcase
<!-- Including any screenshots of the changes. -->

### Changelog

| Language   | Changelog |
| ---------- | --------- |
| 🇺🇸 English |       |
| 🇨🇳 Chinese |       |

### Docs

| Language   | Link |
| ---------- | --------- |
| 🇺🇸 English |  <!-- [Title](link) -->    |
| 🇨🇳 Chinese |  <!-- [标题](link) -->  |

### Checklists
- [ ] All changes have been self-tested and work as expected
- [ ] Test cases are updated/provided or not needed
- [ ] Doc is updated/provided or not needed
- [ ] Component demo is updated/provided or not needed
- [ ] Changelog is provided or not needed
- [ ] Request a code review if it is necessary
```

## Output

Report branch name, commit hash/message, pushed remote branch, and PR link.
