## Overview

In this task, you will write a Playwright test that verifies **search and clear** behaviour on the books page.

## Goal

- Search for an existing book (for example “Clean Code”) and assert that it appears in the results.
- Clear the search and assert either an empty result list or a defined “no results” state.
- Optional: encapsulate search helpers in the `BooksPage` Page Object.

## Steps

1. Reuse or create a spec file for search, e.g. `books-search.spec.ts`.
2. Navigate to `/books`.
3. Fill the search input with an existing title and trigger search (button, Enter key, etc.).
4. Assert that the matching book appears in the results.
5. Clear the search (dedicated button or clearing the input).
6. Assert the empty / “no results” behaviour defined by the application.

## Optional: POM helpers

- Add methods like `search(term: string)` and `clearSearch()` to your `BooksPage`.
- Keep tests focused on **behavioural assertions** while the POM hides locators.

## Success Criteria

- [ ] A test searches for a known book and the result is visible.
- [ ] After clearing, the UI shows an empty or “no results” state according to the app.
- [ ] The test passes consistently without manual sleeps.
- [ ] (Optional) Search logic is encapsulated in `BooksPage`.
