# GitHub Copilot Instructions

## Engineering Behavior Guidelines

### Think Before Coding

- Do not assume unclear requirements
- If requirements are ambiguous, ask clarifying questions
- Surface tradeoffs instead of silently choosing an approach
- Prefer the simplest valid implementation
- If a simpler approach exists, recommend it

---

### Simplicity First

- Implement only what was requested
- Avoid speculative abstractions
- Avoid premature optimization
- Do not add configurability unless explicitly required
- Prefer minimal and maintainable code
- Match the complexity level of the existing codebase

Before implementing, ask:

> "Is this the simplest solution that correctly solves the problem?"

---

### Surgical Changes

When editing existing code:

- Modify only what is necessary for the requested task
- Do not refactor unrelated code
- Do not change unrelated formatting or structure
- Match the existing project style and architecture
- Remove only unused code introduced by your own changes

If unrelated issues are discovered:

- mention them
- do not automatically fix them unless requested

Every changed line should directly relate to the requested task.

---

### Goal-Driven Execution

For implementation tasks:

1. Define the expected outcome
2. Implement the minimal required changes
3. Verify the result
4. Ensure no regressions are introduced

For bug fixes:

- reproduce the issue when possible
- implement the fix
- verify the issue is resolved

For refactors:

- preserve existing behavior
- avoid unnecessary architectural changes
- run and validate existing tests when available
- otherwise verify functionality through application behavior, logic analysis, and existing flows

---

## Commit Message Rules (Conventional Commits)

When generating git commit messages, you MUST follow the Conventional Commits specification.

---

### Commit Message Format

```text
<type>(optional scope): <short description>

[optional body]

[optional footer]
```

---

### Allowed Commit Types

- feat → new feature
- fix → bug fix
- docs → documentation changes
- style → formatting changes only
- refactor → code restructuring without behavior change
- test → adding or updating tests
- chore → maintenance tasks
- ci → CI/CD related changes
- build → dependency or build changes
- perf → performance improvements

---

### Commit Message Rules

- ALWAYS start with a valid commit type
- Use lowercase for commit type and description
- Add a scope when applicable (e.g., auth, api, ui, cart, checkout)
- Keep the subject line concise (max 72 characters)
- Use imperative tone:
  - add
  - fix
  - update
  - refactor
- DO NOT use vague commit messages such as:
  - update code
  - fix stuff
  - changes
  - miscellaneous fixes

---

### Breaking Changes

If a commit introduces a breaking change, use ONE of the following approaches:

#### Option 1: Use `!` in the header

```text
feat(api)!: change authentication response structure
```

#### Option 2: Use `BREAKING CHANGE:` in the body or footer

```text
feat(api): update authentication response structure

BREAKING CHANGE: authentication response format changed and is not backward compatible
```

---

## Code Quality Expectations

- Prefer readability over cleverness
- Keep functions focused and maintainable
- Avoid deeply nested logic when possible
- Follow existing architectural patterns
- Use meaningful and explicit naming
- Avoid duplicated logic
- Avoid introducing unnecessary dependencies

---

## Pull Request Review Handling

After Pull Request review feedback is received:

- Read all review comments and suggestions carefully
- Evaluate whether the feedback is valid within the current project context
- Consider:
  - business requirements
  - architecture consistency
  - maintainability
  - performance implications
  - existing project conventions

### Decision Rules

If feedback is valid:

- apply the improvement carefully
- ensure no regressions are introduced

If feedback is invalid or conflicts with project requirements:

- do not apply it automatically

Never blindly apply all review suggestions.

---

## Frontend Engineering

For frontend architecture, React patterns, Next.js App Router conventions, TypeScript standards, Tailwind rules, accessibility, and performance requirements, follow:

- `skills/senior-frontend-engineer/SKILL.md`

---

## Important

- These instructions apply to all generated code, refactors, and commit messages
- Prefer clarity, maintainability, and correctness over unnecessary complexity
- Do not introduce unrelated changes
- Keep implementations aligned with the existing project architecture and coding style
