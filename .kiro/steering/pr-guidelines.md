# Pull Request Guidelines

## Branch Naming
- `feature/` — new features (e.g., `feature/add-leaderboard`)
- `fix/` — bug fixes (e.g., `fix/score-reset-on-game-over`)
- `refactor/` — code refactoring with no behavior change
- `docs/` — documentation-only changes
- `chore/` — maintenance tasks (dependencies, configs)

## Commit Messages
- Use **Conventional Commits** format: `type: short description`
- Types: `feat`, `fix`, `refactor`, `docs`, `chore`, `test`, `style`, `perf`
- Keep the subject line under 72 characters
- Use imperative mood (e.g., "add feature" not "added feature")
- Example: `feat: add high score persistence to localStorage`

## PR Title
- Follow the same Conventional Commits format as commit messages
- Be concise but descriptive enough to understand the change at a glance
- Example: `feat: add multiplayer support with WebSocket integration`

## PR Description Template
Every PR description should include:

1. **Summary** — What does this PR do and why?
2. **Changes** — Bullet list of key changes made
3. **Testing** — How was this tested? (manual steps, automated tests, screenshots)
4. **Screenshots** — Include before/after screenshots for any UI changes
5. **Notes** — Any additional context, trade-offs, or follow-up items

## PR Best Practices
- Keep PRs **small and focused** — one logical change per PR
- Ensure all existing tests pass before requesting review
- Self-review your own diff before marking as ready
- Link related issues using `Closes #123` or `Fixes #123`
- Resolve all review comments before merging
- Rebase on the latest base branch before merging to avoid conflicts

## Review Expectations
- PRs should be reviewed within 24 hours
- Reviewers should provide constructive, actionable feedback
- Approve only when all comments are addressed and CI passes
