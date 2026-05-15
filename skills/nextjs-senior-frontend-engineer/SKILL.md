---
name: senior-frontend-engineer
description: "Senior frontend engineer decision gates and checklists for this project. Use when: designing a component, reviewing code, optimizing performance, auditing accessibility, writing TypeScript, or before shipping any UI change. Triggers: component architecture, code review, Core Web Vitals, a11y audit, TypeScript types, test strategy, PR checklist, frontend quality."
argument-hint: "component name, feature, or area to review"
---

# Senior Frontend Engineer

Quality gates for every stage of frontend work in this project. Apply before shipping.

## Project Stack

| Layer          | Tool                                                   |
| -------------- | ------------------------------------------------------ |
| Framework      | Next.js 16 (App Router, Turbopack)                     |
| UI             | shadcn/ui + Radix UI primitives                        |
| Styling        | Tailwind CSS v4                                        |
| Class merging  | `clsx` + `tailwind-merge` via `cn()` in `lib/utils.ts` |
| Carousel       | Embla Carousel                                         |
| Themes         | `next-themes`                                          |
| Icons          | `lucide-react`                                         |
| Language       | TypeScript 5                                           |
| Linting/Format | ESLint + Prettier + `prettier-plugin-tailwindcss`      |

## Project Structure Conventions

```
app/           # Next.js App Router — layouts, pages, globals.css
components/    # Reusable UI components (shadcn primitives in ui/, shared in root)
  home/        # Home-page-specific components
  ui/          # shadcn/ui primitives — do NOT hand-edit generated files
constants/     # Static data (brands, categories, products, etc.) — no logic
hooks/         # Custom React hooks
providers/     # Context providers (ThemeProvider, etc.)
services/      # API/data fetching layer
types/         # Shared TypeScript types
views/         # Page-level view compositions (home/, etc.)
```

**Rule:** Pages own layout; `views/` own composition; `components/` own rendering; `services/` own data.

---

## 1. Component Design Gates

Before writing a component:

- [ ] **Always check `components/ui/` (shadcn) first** — if a primitive exists there, use it. Never replace a shadcn component with a native HTML equivalent (e.g., use `<Checkbox>` not `<input type="checkbox">`, `<Select>` not `<select>`, `<Switch>` not `<input type="checkbox">`).
- [ ] If the needed shadcn primitive is missing, add it via `npx shadcn@latest add <component>` before writing a custom implementation.
- [ ] Does it belong in `components/home/` (page-specific) or `components/` (shared)?
- [ ] Server Component by default. Add `"use client"` only when hooks/browser APIs are required.
- [ ] Prefer Server Components for:
  - data fetching
  - rendering static content
  - backend integration
  - SEO-sensitive content
- [ ] Use Client Components only for:
  - event handlers
  - browser APIs
  - local interactive state
  - animations requiring client-side execution
- [ ] Do not convert components to Client Components unless necessary.
- [ ] Define the TypeScript interface before writing JSX. Check `types/` for existing shared types.
- [ ] Is static data driving this? Put it in `constants/` — never hardcode arrays inline in JSX.

Architecture rules:

- One component per file; filename = exported component name (kebab-case file, PascalCase export).
- Keep components under ~150 lines. Extract hooks or sub-components when they grow.
- Prefer composition (`children`, `asChild`, slot props) over boolean configuration props.
- Data fetching belongs in `services/` + `views/` or `app/` pages — not in presentational components.

---

## 2. TypeScript

- [ ] No `any`. Use `unknown` + type guards or define a type in `types/`.
- [ ] Shared types live in `types/index.ts` or `types/product.ts` — not scattered across components.
- [ ] `as` casts only at API/system boundaries — never inside component or business logic.
- [ ] Utility types (`Pick`, `Omit`, `Partial`, `NonNullable`) over hand-rolled duplicates.
- [ ] Run `npm run typecheck` before opening a PR.

---

## 3. Styling (Tailwind v4)

- [ ] Use `cn()` from `lib/utils.ts` for all conditional/merged class strings — never string interpolation.
- [ ] Tailwind scale tokens only (`p-4`, `text-sm`); arbitrary values only when the design spec requires it.
- [ ] Responsive variants mobile-first (`sm:`, `md:`, `lg:`).
- [ ] Dark-mode variants where specified (`dark:`). The `ThemeProvider` in `providers/` is already wired.
- [ ] No inline `style={}` for anything achievable with Tailwind.
- [ ] Run `npm run format` (Prettier + `prettier-plugin-tailwindcss`) before committing to auto-sort classes.

---

## 4. Performance & Core Web Vitals

**LCP**

- [ ] Hero/above-fold images use `<Image priority />`. No `priority` on below-fold images.
- [ ] Fonts loaded with `next/font` — never raw `@import` in CSS.

**CLS**

- [ ] Every `<Image>` has explicit `width` + `height` or uses `fill` with a sized parent container.
- [ ] Carousels (Embla) initialized with reserved height to prevent layout shift.

**Bundle**

- [ ] Dynamic `import()` for heavy features not needed on first paint.
- [ ] `lucide-react` icons imported individually (`import { ShoppingCart } from 'lucide-react'`), never as a namespace.

---

## 5. Accessibility (a11y)

- [ ] All interactive elements keyboard-operable (`Tab`, `Enter`, `Space`, `Esc`).
- [ ] Every `<img>` / `<Image>` has a descriptive `alt`; decorative → `alt=""`.
- [ ] Color contrast ≥ 4.5:1 normal text, 3:1 large text (WCAG AA).
- [ ] Focus ring never removed without a replacement style.
- [ ] Carousel (Embla) has `aria-label` on the container and `aria-roledescription="slide"` on items.
- [ ] Icon-only buttons have `aria-label`. Lucide icons inside buttons get `aria-hidden="true"`.
- [ ] Form inputs associated with a `<label>` via `htmlFor`/`id` or `aria-label`.
- [ ] Heading hierarchy (`h1` → `h2` → `h3`) never skips a level across views.

---

## 6. Next.js (App Router) Specifics

- [ ] Read `node_modules/next/dist/docs/` before using any Next.js API — do not rely on training data.
- [ ] Default to Server Components. Move to Client only when needed.
- [ ] `views/` components that need client state must be wrapped in a Client Component boundary — not the entire view.
- [ ] `generateMetadata` on every public page in `app/`.
- [ ] Route handlers in `app/api/` validate and sanitize all input before processing.
- [ ] Use `loading.tsx` / `error.tsx` segments for streaming UI and error boundaries.

---

## 7. Code Review Gates

**Before opening a PR:**

- [ ] `npm run build` passes — zero TypeScript errors, no broken imports.
- [ ] `npm run lint` clean.
- [ ] `npm run typecheck` clean.
- [ ] No `console.log`, leftover `TODO`, or commented-out code (or add a ticket reference).
- [ ] New static data added to `constants/` with proper typing.
- [ ] New shared types added to `types/`, not inline in a single component file.
- [ ] New reusable component placed in `components/` (or `components/home/` if page-specific), not buried in `views/`.

---

## Quick Ship Checklist

```
[ ] npm run typecheck  — passes
[ ] npm run lint       — clean
[ ] npm run build      — passes
[ ] Responsive: 375px (mobile) and 1440px (desktop) checked
[ ] Keyboard navigable — tabbed through new UI
[ ] Contrast checked for any new colors
[ ] No layout shift introduced (images have dimensions)
[ ] PR description explains WHY, not just what
```
