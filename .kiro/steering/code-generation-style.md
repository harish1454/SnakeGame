# Code Generation Style Guidelines

## Comments & Documentation
- Always add meaningful inline comments explaining **why**, not just **what**
- Add JSDoc/docstring comments for all functions and classes
- Include a brief file-level comment at the top of each new file describing its purpose
- Use `// TODO:` for planned improvements and `// FIXME:` for known issues

## Code Style Preferences
- Prefer **functional patterns** over imperative where appropriate (map, filter, reduce over for-loops)
- Use **descriptive variable and function names** — avoid single-letter variables except in trivial loops
- Keep functions small and focused — each function should do one thing well
- Prefer **early returns** to reduce nesting depth
- Use **const** by default; only use `let` when reassignment is necessary

## Structure & Organization
- Follow the **Single Responsibility Principle** — one module/file per concern
- Group related functions together with section comments
- Place helper/utility functions at the bottom of the file or in a separate utils module
- Keep imports organized: external libraries first, then internal modules, then relative imports

## Error Handling
- Always handle errors explicitly — no silent failures
- Provide meaningful error messages that help with debugging
- Use try/catch for async operations and validate inputs at function boundaries

## Best Practices
- Write code that is **readable first**, optimized second
- Avoid magic numbers — use named constants
- Prefer **composition over inheritance**
- Keep side effects contained and predictable
- Follow DRY (Don't Repeat Yourself) but don't over-abstract prematurely
