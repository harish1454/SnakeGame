# Contributing to SnakeGame

Thank you for your interest in contributing! Please follow these guidelines to keep the project organized and maintainable.

## Branch Policy

- **Always create a new branch** for your changes — never commit directly to `main`
- Use descriptive branch names following this convention:
  - `feature/<description>` — new features
  - `fix/<description>` — bug fixes
  - `refactor/<description>` — code refactoring
  - `docs/<description>` — documentation changes
  - `chore/<description>` — maintenance tasks

## Pull Request Workflow

1. **Fork the repo** (external contributors) or create a new branch (collaborators)
2. **Create a feature branch** from `main`:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes** — keep commits small and focused
4. **Push your branch**:
   ```bash
   git push origin feature/your-feature-name
   ```
5. **Open a Pull Request** into `main`

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```
type: short description

Optional longer description
```

**Types:** `feat`, `fix`, `refactor`, `docs`, `chore`, `test`, `style`, `perf`

**Examples:**
- `feat: add pause functionality`
- `fix: correct score reset on game over`
- `docs: update README with new screenshots`

## PR Requirements

- PRs must target the `main` branch
- Include a clear description of what changed and why
- Add screenshots for any UI changes
- Ensure existing functionality is not broken
- Keep PRs focused — one logical change per PR

## Code Style

- Add meaningful comments explaining **why**, not just **what**
- Use descriptive variable and function names
- Keep functions small and focused
- Use `const` by default; `let` only when reassignment is needed
- No magic numbers — use named constants

## Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/harish1454/SnakeGame.git
   cd SnakeGame
   ```
2. Open `index.html` in your browser to run the game
3. Make your changes and test locally before submitting

## Questions?

Open an issue if you have questions or want to discuss a feature before implementing it.
