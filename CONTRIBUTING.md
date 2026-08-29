# Contributing Guide

## Branch Workflow

**Always create a new branch for your work.** Never commit directly to `main`.

### Steps

1. **Create a new branch** from `main`:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b <branch-name>
   ```

2. **Branch naming convention:**
   - Features: `feature/<short-description>`
   - Bug fixes: `fix/<short-description>`
   - Docs: `docs/<short-description>`

3. **Make your changes** and commit with clear, descriptive messages:
   ```bash
   git add <files>
   git commit -m "Brief description of the change"
   ```

4. **Push your branch** and open a Pull Request:
   ```bash
   git push origin <branch-name>
   ```

5. **Open a Pull Request** targeting `main` for review.

## Rules

- **Never push directly to `main`.**
- Keep PRs focused — one feature or fix per branch.
- Ensure your code works before opening a PR.
- Delete your branch after it has been merged.
