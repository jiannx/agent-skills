---
name: nocobase-pr-workflow
description: Create a NocoBase task or conventional branch, commit current fix, push the remote branch, and create a pull request with the repository template. Use when the user asks to create a branch, commit, push, or open a PR for NocoBase work, with or without a taskid; includes required base-branch confirmation, changelog drafting, PR duplicate checks, and `gh pr create`.
---

# NocoBase PR Workflow

Use this skill after a NocoBase change is implemented and the user wants the branch, commit, push, and PR flow handled.

For PR creation only on an already committed branch, skip the branch/commit steps but still follow the PR creation, base-branch, changelog, and safety rules below.

## Target Repository

Do not assume the workspace root is the git repo to operate on.

- First identify the target git repository for the requested change.
- Prefer the deepest git root that contains the relevant changed files or the user-specified path.
- Some NocoBase plugins live in independent repositories nested under the main workspace, for example `packages/pro-plugins` or `packages/pro-plugins/@nocobase/plugin-email-manager`.
- Use `git -C <candidate-path> rev-parse --show-toplevel` to resolve the repo root from the most specific relevant path.
- Run all `git`, `gh`, and `.github/pull_request_template.md` checks against that target repo root, not blindly against the monorepo root.
- If the requested changes span multiple git repositories, stop and ask the user which repository to handle first. Do not mix branch, commit, push, or PR operations across repos in one pass.

## Inputs

- Optional `taskid`; if the user sends a plain number, treat it as the task ID.
- Optional target path or plugin path. Infer it from changed files or the user's request when possible.
- Base branch for the PR. Ask `Which base branch should I use for the PR?` before creating the PR unless the user already answered it in the current conversation. Do not silently default to `main` or `next`.
- Current base branch for creating a new work branch must be `main`, `next`, or `develop`. Do not invent other local base branches.
- PR type: bug fix, improvement, new feature, or other. Infer from context when obvious; ask only if risky.

## Workflow

1. Identify the target repository root before any git operation.
2. Confirm the target repo state with `git status --short` and current branch with `git branch --show-current`.
3. If creating a new branch, ensure the current branch in the target repo is `main`, `next`, or `develop`. If not, stop and ask which allowed local base branch to use.
4. Review the diff in the target repo and identify the relevant files to stage.
5. Draft the full execution plan before changing git state:
   - target repository root
   - base branch and new branch name
   - files that will be staged
   - concise conventional commit message generated from the actual changes
   - PR title following the PR title rules below
   - PR body using `.github/pull_request_template.md`
6. Show the full execution plan and final PR title/body together, then ask the user to confirm. Do not create the branch, stage files, commit, push, or create/publish the PR before this confirmation.
7. After confirmation, create or switch to the confirmed branch from the current base branch in the target repo.
8. Stage only the confirmed relevant files, commit with the confirmed message, and push with `git push -u origin <branch-name>` from the target repo.
9. Before creating the PR, confirm the PR base branch from the user if not already confirmed in the current conversation.
10. Read `.github/pull_request_template.md` from the target repo, compare `HEAD` with `origin/<base>`, check for existing PRs for the branch, then create the PR with `gh pr create`.
11. Output the target repo, pushed branch, base branch, PR link, and any warnings.

## PR Creation Workflow

Run this sequence from the target repo root:

1. Check branch and local state:
   ```bash
   git branch --show-current
   git status --short
   ```
2. Confirm the branch contains the intended commit before pushing:
   ```bash
   git log --oneline -5
   ```
3. Push the current branch to `origin` if needed:
   ```bash
   git push -u origin <branch-name>
   ```
4. Read the repository PR template:
   ```bash
   sed -n '1,220p' .github/pull_request_template.md
   ```
5. Compare the branch against the confirmed PR base:
   ```bash
   git diff --stat origin/<base>...HEAD
   git log --reverse --format=%s origin/<base>..HEAD
   ```
6. Check whether a PR already exists for the branch:
   ```bash
   gh pr status
   ```
7. Create the PR:
   ```bash
   gh pr create --base <base> --head <branch-name> --title "<title>" --body-file <body-file>
   ```

## Branch Rules

- If `taskid` is provided, branch name must be `task-{taskid}`.
- If `taskid` is not provided, generate a conventional branch name from the actual changes and PR title, using `<type>/<short-kebab-summary>`, for example `fix/restore-filter-operator`, `feat/add-calendar-view`, or `docs/update-pr-workflow`.
- Branch type should align with the change: `fix`, `feat`, `docs`, `refactor`, `test`, `chore`, `perf`, `build`, or `ci`.
- Keep generated branch names lowercase ASCII, kebab-case, concise, and free of spaces or punctuation other than `/` and `-`.
- For bug fixes or non-feature modifications, base should normally be `main`.
- For new features or API modifications, base should normally be `next`.
- `develop` is allowed only when the repo/user is already on it or explicitly requests it.
- Never rebase, reset, or discard local changes unless the user explicitly asks.

## Commit Rules

- Base the commit message on the actual diff.
- Prefer conventional commits, for example `fix: ...`, `feat: ...`, `chore: ...`.
- Confirm the commit message together with the branch, staged files, and PR content before committing.
- Keep one task in one commit unless the existing diff clearly contains unrelated work; then ask before staging.

## PR Title Rules

- PR title must use a conventional commit-style prefix, such as `fix: ...`, `feat: ...`, `chore: ...`, `docs: ...`, or `refactor: ...`.
- If the current change belongs to a specific plugin, include the plugin name as the scope, for example `feat(kanban): ...`, `fix(calendar): ...`, or `chore(auth): ...`.
- Infer the plugin scope from changed paths, package names, or clear domain context. Omit the scope when the change is repo-wide or no specific plugin is evident.
- Keep the PR title concise and aligned with the confirmed commit message unless the PR needs a broader summary.

## PR Template

Always read `.github/pull_request_template.md` from the target repository and keep its structure intact. Fill only fields that have clear evidence from the task, diff, or user-provided context. Leave unavailable links, screenshots, docs, or issue references blank instead of inventing placeholders.

If the target repo does not contain `.github/pull_request_template.md`, say so clearly and ask the user whether to continue with a manually prepared PR body.

Before `gh pr create`, confirm no template TODOs or accidental placeholder text remain in fields you filled.

## Changelog Rules

Write changelog entries from the user-visible behavior, not from the commit message.

- Do not reuse the commit message.
- Do not write `fix(xxx): ...`, `feat(xxx): ...`, or similar commit-style prefixes.
- Describe the actual user-visible change.
- Keep English and Chinese lines aligned in meaning.
- Leave changelog blank only when the change is clearly internal and no changelog is needed.

Good examples:

- `Fixed belongsTo queries when the source collection uses a non-primary filterTargetKey as its unique identifier`
- `修复源表使用非主键 filterTargetKey 作为唯一标识时 belongsTo 查询报错的问题`

Bad example:

- `fix(database): support belongsTo query by filter target key`

## Safety Checks

Before `git push`:

- Confirm the branch contains the intended commit.
- Warn if there are uncommitted local files; do not include unrelated files.
- Confirm the push is happening from the intended target repo, especially when working under nested plugin paths.

Before `gh pr create`:

- Confirm the PR base branch from the user.
- Confirm the target repo root being used.
- Confirm `.github/pull_request_template.md` was read.
- Confirm the branch was compared with `origin/<base>`.
- Confirm no PR already exists for the branch, or report the existing PR instead of creating a duplicate.
- Confirm no template TODOs remain in filled content.
- State clearly if tests were not run.

## Output

Report target repo path, branch name, commit hash/message when a commit was created, pushed remote branch, base branch used, PR link, and any remaining warnings.
