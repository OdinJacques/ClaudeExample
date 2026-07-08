# Write Part 2's Exercise 2 test — pressPercent()

## Context
`requirements.md` Part 2 asks students to replace `pending(...)` placeholders in `src/app/app.component.spec.ts` with real assertions. Part 1 (including `pressPercent()`) is already implemented and merged into `ClaudeExample:main`. This plan covers just the "Exercise 2" test — the one still-pending test that verifies `pressPercent()` actually works end-to-end through the UI, not just that it doesn't throw.

## What to change
File: [src/app/app.component.spec.ts:225-229](src/app/app.component.spec.ts:225)

Current pending test:
```ts
// 🎯 YOUR TURN — after implementing EXERCISE 2:
//   display shows "50" → press % → display should show "0.5"
it('should divide the displayed number by 100 when % is pressed', () => {
  pending('Student exercise — implement pressPercent() first, then write this test');
});
```

Replace the `pending(...)` body with a real assertion, following the exact pattern already used by the passing "should add two numbers correctly" test at [app.component.spec.ts:100-109](src/app/app.component.spec.ts:100) (press digits via `component.pressDigit(...)`, call the method under test, run `fixture.detectChanges()`, then read `.display__value` and assert with `.toBe(...)`):

```ts
it('should divide the displayed number by 100 when % is pressed', () => {
  component.pressDigit('5');
  component.pressDigit('0');
  component.pressPercent();
  fixture.detectChanges();

  const displayEl = fixture.nativeElement.querySelector('.display__value');
  expect(displayEl.textContent.trim()).toBe('0.5');
});
```

This matches the spec's own hint (`display shows "50" → press % → display should show "0.5"`) and reuses the existing `displayEl`/`textContent.trim()` assertion style already used throughout the file — no new patterns or helpers introduced.

## Verification
1. Run the test suite in single-run mode (the existing `karma.conf.js` defaults to `singleRun: false` / `autoWatch: true`, which would hang in an automated shell — run with an override instead): `npx ng test --watch=false --browsers=ChromeHeadless`.
2. Confirm the previously-pending test now passes, and that no other test regresses (in particular the two existing `✅` tests for the percent button/no-throw just above it, and the full suite count).
3. Optionally cross-check manually in the running app (`npm start` via the preview tool): type `50`, press `%`, confirm the display shows `0.5`, matching what the new test asserts.

## Out of scope
- The other still-`pending` tests in Part 2 (multi-digit display, decimal point handling, clear-cancels-operation, subtraction, multiplication, division decimal, chained operations, Exercise 1 tests) — separate, follow-up work.
- Part 3 (`CLAUDE.md`).
- No implementation code (`app.component.ts`) changes — `pressPercent()` is already correct and verified.