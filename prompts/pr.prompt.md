---
description: "Create/update feature branch, commit all local changes with Conventional Commits, push, and open draft PR with gh CLI"
name: "pr"
agent: "agent"
---

Create draft pull request from current working state.

Goal:
- Analyze all changes since branch point from main/master, including uncommitted local changes.
- Ensure branch, commit, push, and draft PR are completed end-to-end.

Execution rules:
1. Resolve base branch:
- Prefer main if it exists on origin.
- Else use master if it exists on origin.
- If neither exists, stop and explain.

2. Resolve working branch:
- If current branch is main or master, create new feature branch and switch to it.
- Branch name format: <type>/<short-kebab-summary> (example: feat/add-pr-automation).
- If already on non-main/master branch, keep current branch.

3. Stage and commit local changes:
- Include all local tracked/untracked changes relevant to workspace.
- Generate Conventional Commit message from actual diff (format: type(scope): summary, scope optional).
- If no changes to commit, continue without creating new commit.

4. Push branch:
- Push branch to origin.
- If first push for branch, set upstream.

5. Build PR metadata from diff:
- Compute merge-base with chosen base branch.
- Summarize what changed since merge-base through current HEAD (including newly committed local changes).
- Identify risks, migration notes, rollout concerns, testing gaps, and breaking behavior.

6. Create draft PR with gh CLI:
- Use gh pr create.
- Create as draft.
- Title must follow Conventional Commits style.
- Body must include:
	- Summary of changes
	- Risks
  - Relevant notes
- Target base branch from step 1.
- Head branch is current feature branch.

Output format:
- Branch used/created
- Push result
- PR URL

Safety and quality:
- Do not use force push.
- Do not rewrite history.
- If git or gh auth/config blocks progress, report exact blocker and next fix command.
