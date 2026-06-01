# Web App Agent Guide

This is the main operating manual for AI agents working in `apps/web`.

Agents should follow this document before implementing product behavior.

---

## Read Order

Before coding, read these files in order:

1. `prd.md`
2. `progress-tracker.md`
3. `feature-specs/00-index.md`
4. The relevant feature spec in `feature-specs/`

If no feature spec exists for the requested work, create one before implementing unless the user explicitly says the change is tiny.

---

## Project Assumptions

This app uses:

* Next.js App Router
* React
* TypeScript
* Tailwind CSS
* shadcn/ui or shared UI components
* Zod for validation
* Feature-based architecture

Adjust implementation to the actual repo if these assumptions conflict with existing code.

---

## Core Workflow

For any meaningful feature, bug fix, or refactor:

1. Read the docs listed above.
2. Confirm the relevant feature spec.
3. Implement one task at a time.
4. Keep changes scoped to the requested feature.
5. Verify the work.
6. Update `progress-tracker.md`.
7. Update `feature-specs/00-index.md` if feature status changed.

Do not implement multiple feature tasks at once unless the user explicitly asks.

---

## Feature Spec Rules

Feature specs live in:

```txt
apps/web/docs/feature-specs/
```

Each feature should have one spec file:

```txt
01-auth-signup.md
02-dashboard-shell.md
03-ai-chat.md
```

Every feature spec should include:

* Goal
* User story
* Requirements
* Out of scope
* Proposed file structure
* Acceptance criteria
* Implementation tasks
* Verification steps

The feature spec is the source of truth for the feature.

If the prompt conflicts with the feature spec, ask the user before proceeding.

---

## Progress Tracker Rules

The progress tracker lives here:

```txt
apps/web/docs/progress-tracker.md
```

Before starting:

* Read the current goal.
* Check what is already in progress.
* Check open questions.

After meaningful work:

* Move completed items from `In Progress` to `Completed`.
* Update `Current Goal`.
* Update `Next Up`.
* Add unresolved issues to `Open Questions`.
* Add short notes that would help the next session resume.

Do not treat the task as complete until the tracker has been updated.

---

## Folder Structure

Use feature-based organization.

Preferred pattern:

```txt
apps/web/
  app/
    signup/
      page.tsx
  features/
    auth/
      components/
      hooks/
      actions/
      schemas/
      types.ts
  components/
    layout/
    shared/
  lib/
```

Routes and pages should stay thin.

Feature implementation should live under:

```txt
features/<feature-name>/
```

---

## Route Rules

For App Router pages:

* Keep `page.tsx` focused on layout and data boundaries.
* Move UI into feature components.
* Move form actions, validation, and business logic out of the route.
* Prefer Server Components by default.
* Use `"use client"` only when browser APIs, local state, effects, or event handlers are required.

Example:

```txt
app/signup/page.tsx
features/auth/components/signup-form.tsx
features/auth/actions/signup.ts
features/auth/schemas/signup-schema.ts
```

---

## TypeScript Rules

* Use strict TypeScript.
* Avoid `any`.
* Prefer `unknown` with narrowing when input shape is uncertain.
* Use explicit types at API and feature boundaries.
* Infer internal types when TypeScript can do so clearly.
* Use Zod schemas for external input validation.
* Export types from feature-local `types.ts` only when shared by multiple files.

---

## React Rules

* Keep components focused on one responsibility.
* Prefer composition over deeply nested conditionals.
* Extract reusable stateful logic into `use-*` hooks.
* Avoid large components that mix UI, data fetching, validation, and mutation logic.
* Keep server state and client state separate.
* Avoid unnecessary global state.
* Use existing patterns before introducing new abstractions.

---

## Styling Rules

* Use Tailwind utilities or existing design-system components.
* Do not use random hex colors in feature code.
* Prefer semantic tokens and existing variants.
* Keep class names readable.
* Extract repeated UI patterns into components when they appear repeatedly.

Do not create a new design pattern if an existing component already solves the problem.

---

## Engineering Preferences

Optimize for code that is easy to read, test, and change later.

Follow these principles:

* SOLID where practical
* Single responsibility
* Meaningful names
* Clear boundaries
* Composition over inheritance
* Encapsulation of implementation details
* Readability over cleverness

Avoid:

* Premature abstraction
* Overly generic helpers
* Large “god” components
* Duplicated business logic
* Deeply nested conditionals
* Unrelated refactors during feature work

Duplication rule:

* Duplication once is acceptable.
* Duplication twice is a signal.
* Abstract on the third clear use case.

---

## Comments

Prefer self-documenting code, but comments are allowed when they explain why something exists.

Good comments explain:

* Non-obvious decisions
* External constraints
* Workarounds
* Security concerns
* Complex algorithms

Avoid comments that simply repeat what the code already says.

---

## Testing Rules

When adding or changing logic:

* Add tests when the repo already has a test pattern for that area.
* Test important edge cases.
* Prefer behavior-focused tests over implementation-detail tests.
* For bug fixes, add a failing test first when practical.

Do not invent a new testing framework.

Use the existing test setup.

---

## Validation Commands

When code changes, run the smallest useful validation command.

Common commands:

```sh
pnpm typecheck
pnpm test
pnpm build
```

If the repo uses different commands, follow the actual `package.json`.

For documentation-only changes, validation may not be required.

---

## Dependency Rules

Do not add new dependencies unless:

* The feature spec requires it
* The existing stack cannot reasonably solve the problem
* The user approves it

Before adding a dependency, prefer:

1. Existing utilities
2. Platform APIs
3. Small local helper
4. New package only if justified

---

## API and Server Action Rules

For mutations:

* Validate input server-side.
* Return structured success/error results.
* Do not leak raw errors to the UI.
* Keep auth checks close to the server boundary.
* Keep business rules out of UI components.

For external input:

* Use Zod or existing validation utilities.
* Never trust client-side validation alone.

---

## Accessibility Rules

For UI work:

* Use semantic HTML.
* Ensure form fields have labels.
* Ensure buttons have accessible names.
* Preserve keyboard navigation.
* Use visible focus states.
* Avoid clickable divs when a button or link is appropriate.

---

## Protected Areas

Do not modify these unless explicitly asked:

* Generated files
* Build output
* Vendor code
* Lockfiles, unless dependency changes require it
* Shared design-system primitives, unless the task is specifically about the design system

---

## Conflict Resolution

When instructions conflict, follow this order:

1. User's explicit instruction
2. Relevant feature spec
3. `progress-tracker.md`
4. `prd.md`
5. Existing code patterns
6. This document

Reference notes or copied online rules do not override project requirements.

---

## Before Finishing Checklist

Before saying the task is done:

* Relevant feature spec was followed
* Code stayed within scope
* TypeScript errors were not introduced
* Progress tracker was updated
* Feature index was updated if status changed
* Open questions were recorded if something remains unresolved
