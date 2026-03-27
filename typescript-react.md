---
description: TypeScript/React patterns — Mantine, TanStack, FSD, Zod
globs: "**/*.ts,**/*.tsx,**/*.js,**/*.jsx"
alwaysApply: false
---

# TypeScript & React Patterns

## React Components

- Functional components only, typed with `React.FC` or explicit prop types
- Colocate hook logic: extract custom hooks when a component exceeds ~30 lines of logic
- Prefer controlled components; uncontrolled only with `useRef` when performance demands it
- Memoize with `React.memo`/`useMemo`/`useCallback` only when profiling shows a need — not by default

## Mantine UI

- Use Mantine components as the primary UI library — don't reimplement what Mantine provides
- Theming via `MantineProvider` and `theme` object; avoid inline `style` props for anything theme-addressable
- Use `@mantine/form` with Zod resolver for form validation (`zodResolver`)
- Responsive props: prefer Mantine's responsive object syntax (`{{ base: 'sm', md: 'lg' }}`) over media queries

## TanStack Query

- `useQuery` for reads, `useMutation` for writes — never use `useEffect` for data fetching
- Query keys: structured arrays `['entity', id, filters]`, extract into factory functions
- Stale/cache times: set per-query based on data volatility, don't rely on global defaults
- Invalidate related queries in `onSuccess` of mutations
- Use `select` to transform/derive data in the query, not in the component

## TanStack Router

- File-based routing: route files in `routes/` directory, follow the `__root.tsx` → layout → page convention
- Type-safe search params via Zod schemas in `validateSearch`
- Loaders for data prefetching — colocate loader logic with route definition
- Use `Link` component for navigation, never raw `<a>` tags for internal routes

## Feature-Sliced Design (FSD)

Layer hierarchy (imports flow downward only):

```
app → pages → widgets → features → entities → shared
```

- **shared**: UI kit, libs, API client, types, constants — no business logic
- **entities**: domain models + their UI (cards, rows), data access (query hooks)
- **features**: user actions (create, edit, delete) — compose entities, contain business logic
- **widgets**: composite UI blocks assembling features + entities for a page section
- **pages**: route-level components wiring widgets into layouts
- **app**: providers, routing, global config

Cross-layer rules:
- Never import upward (e.g. `entities` must not import from `features`)
- Each slice has `index.ts` public API — import only from the barrel, never from internal files
- Shared code between slices goes into `shared/` layer

## Zod & API Contracts

- Define Zod schemas as the single source of truth for API shapes
- Derive TypeScript types: `type User = z.infer<typeof userSchema>`
- Use `.transform()` for API response normalization (dates, enums)
- Colocate schemas with the entity/feature that owns them

## Imports

- Barrel imports (`index.ts`) for public API of each FSD slice
- Keep import order: externals → `@your-lib/*` → relative (auto-sorted by linter)
