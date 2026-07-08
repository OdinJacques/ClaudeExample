# Super Calculator

## Project overview
A beginner-friendly Angular 19 calculator built for teaching purposes — Angular
fundamentals, unit testing, and how to work with Claude Code. Students complete
deliberately incomplete methods and tests as coursework (see requirements.md).

## Tech stack
- Angular 19 (standalone components, no NgModules)
- TypeScript 5.7
- Karma + Jasmine for unit tests

## How to run
- Install: `npm install`
- Dev server: `npm start` → http://localhost:4200 (auto-reloads on save)
- Build: `npm run build` → output to `dist/`
- Tests (interactive): `ng test` (opens a Karma/Chrome watch session)
- Tests (single run): `npx ng test --watch=false --browsers=ChromeHeadless`

## Project structure
- `src/app/app.component.ts` — all calculator state and logic (single standalone component)
- `src/app/app.component.html` — template and button grid
- `src/app/app.component.css` — styles, including dark (default) and light theme rules
- `src/app/app.component.spec.ts` — Karma/Jasmine unit tests
- `requirements.md` — the 3-part student assignment spec
- `README.md` — setup/run instructions

## Exercises
Per requirements.md, these areas are intentionally incomplete for students:
1. `pressToggleSign()` — flip the sign of the display value (done)
2. `pressPercent()` — divide the display value by 100 (done)
3. `toggleTheme()` + light-mode CSS — switch dark/light themes (done, but see Known issues)
4. Unit tests in `app.component.spec.ts` marked with `pending(...)` (Part 2) — only the `%` button test is filled in so far; the rest are still placeholders.

## Known issues
- `toggleTheme()`: the `<app-root>` host element's `light` class (bound via
  `host: {'[class.light]': 'isLightMode'}` in the `@Component` decorator) does not
  actually update when `isLightMode` changes, so the outer page background never
  switches to light mode, even though the `.calculator` box itself does. Not yet fixed.

## Coding conventions
- Standalone components only — no NgModules
- Keep all calculator logic in `AppComponent`; no services or state management for this teaching example
- Public methods have short doc comments explaining intent, aimed at students reading the code
- Tests follow the pattern: drive state via component methods (e.g. `pressDigit`, `pressOperator`), call `fixture.detectChanges()`, then assert on `.display__value` text content
