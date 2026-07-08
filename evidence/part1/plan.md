# Verify Part 1 — Missing Methods (toggle sign, percent, theme)

## Context
`requirements.md` Part 1 asks students to implement `pressToggleSign()`, `pressPercent()`, and `toggleTheme()` (plus its light-mode CSS). Reading the code shows all three are already filled in on this branch (`Feat/part1_MissingMethods`, commit "Implement Part 1 exercises: toggle sign, percent, theme toggle"). Rather than re-implementing, the goal now is to **verify the existing implementation is actually correct** against the spec — both by static review and by exercising it live in the browser — and flag anything that doesn't match before moving on to Part 2/3.

## What to check, per exercise

**1. `pressToggleSign()`** — [app.component.ts:148](src/app/app.component.ts:148)
- Logic: `parseFloat(display) * -1`, with a guard so `0 → '0'` (not `'-0'`).
- Verify against spec table: `5→-5`, `-3→3`, `0→0`.
- Also check a decimal case not in the spec table (e.g. `2.5 → -2.5`) since it's the same code path.

**2. `pressPercent()`** — [app.component.ts:161](src/app/app.component.ts:161)
- Logic: `parseFloat(display) / 100`.
- Verify against spec table: `50→0.5`, `200→2`, `1→0.01`.

**3. `toggleTheme()` + light-mode CSS** — [app.component.ts:135](src/app/app.component.ts:135), [app.component.css:146-176](src/app/app.component.css:146)
- Logic: flips `isLightMode` boolean; template already binds `[class.light]` on both `:host` (via `host: {'[class.light]': 'isLightMode'}` in the `@Component` decorator) and `.calculator` div ([app.component.html:26](src/app/app.component.html:26)).
- Button label swap: `🌙 Dark` ↔ `☀️ Light` ([app.component.html:22-24](src/app/app.component.html:22)).
- CSS selectors filled in for `:host.light`, `.calculator.light`, `.display`, `.btn`, `.btn--utility` — spot-check computed colors match the requirements.md suggested values (readable light background, dark text). Note: `.btn--operator`/`.btn--equals` were kept red in light mode too, which isn't in the spec's suggested table but is a reasonable design choice, not a bug — just confirm it's still readable.

## Verification steps
1. **Static review** (already done above by reading the three files) — confirm no obvious logic mismatch against the spec tables.
2. **Run the app and test manually in the browser:**
   - Start the dev server (`npm start` via the preview tool).
   - Type `5`, click `+/-` → expect `-5`; click again → expect `5`.
   - Type `0`, click `+/-` → expect `0` (not `-0`).
   - Type `5`, `0`, click `%` → expect `0.5`.
   - Click the theme toggle button → expect label to flip `🌙 Dark ⇄ ☀️ Light`, and the calculator background/text/button colors to switch to the light palette and back.
   - Use `preview_screenshot`/`preview_inspect` to confirm the light-mode colors are legible (dark text on light background).
3. **Run the existing test suite** (`npm test`) to confirm nothing is broken. Note: this won't independently confirm Part 1 correctness, because the real assertions for Exercise 1/2 (`~200`, `~205`, `~227` in `app.component.spec.ts`) are still `pending(...)` placeholders — that's Part 2's job, out of scope here. Only the smoke tests ("should not throw") currently exercise these methods.
4. Report any mismatch found between actual behavior and the requirements.md tables; if everything matches, confirm Part 1 is complete and ready to move on to Part 2 (tests) or Part 3 (`CLAUDE.md`).

## Out of scope
- Writing the Part 2 unit tests (separate task).
- Creating `CLAUDE.md` for Part 3 (separate task).
- No source files will be modified as part of this verification unless a real bug is found, in which case that would be a follow-up fix to confirm with the user first.