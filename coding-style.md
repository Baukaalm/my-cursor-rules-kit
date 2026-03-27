---
description: Core coding style — functional programming, immutability, composition
alwaysApply: true
---

# Coding Style

## Functional First

- Pure functions, no side effects unless explicitly needed (I/O, DOM)
- Immutable data: `const` only, spread/structuredClone over mutation, `readonly` in types
- Composition over inheritance: pipe small functions, avoid class hierarchies
- `map`/`filter`/`reduce`/`flatMap` over imperative loops
- Expressions over statements: ternary over if/else where readable, early returns

## TypeScript Conventions

- Arrow functions for everything except React components needing display names
- Destructure props, params, and imports
- Strict types (`strict: true`), prefer `type` over `interface`
- Zod for runtime validation, infer types from schemas (`z.infer<typeof schema>`)
- Discriminated unions over optional fields for state modeling
- No `any` — use `unknown` + narrowing; `as` casts only with justification

## File & Function Hygiene

- Functions: ≤20 lines preferred, 40 max; extract helpers aggressively
- Files: ≤200 lines preferred, 400 normal, 800 absolute max — split if larger
- No `console.log` in committed code (use proper logger or remove)
- No comments narrating code — only non-obvious intent, trade-offs, constraints
- DRY: extract repeated patterns into higher-order functions or shared utilities

## Naming

- `camelCase` for variables/functions, `PascalCase` for types/components, `UPPER_SNAKE` for constants
- Boolean variables/props: `is`/`has`/`should`/`can` prefix
- Event handlers: `on` prefix (`onClick`, `onSubmit`)
- Predicates and guards: descriptive verb (`isValid`, `hasPermission`)
