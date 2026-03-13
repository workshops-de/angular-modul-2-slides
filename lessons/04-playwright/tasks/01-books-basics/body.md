## Overview

In this task, you will write your **first Playwright test** for the books page.

## Goal

- Navigate to `/books`.
- Assert that there is at least one `book-item` in the list.
- Optional: refactor the test to use a reusable `BooksPage` Page Object.

## Steps

1. Locate or create a spec file for the books feature, e.g. `books.spec.ts`.
2. Use the injected `page` fixture to call `await page.goto('/books');`.
3. Select the book items, for example via `page.getByTestId('book-item')` or a suitable role.
4. Use `expect` to assert that you have at least one item.
5. Run the test via CLI or VS Code Test Explorer and ensure it passes.

## Optional: Page Object Model

- Extract navigation and locators into a `BooksPage` class with:
  - `goto()`
  - `books()` returning the locator for book items
  - `expectAtLeastOneBook()` using `expect` on the locator count
- Update your test to use `BooksPage` to keep the spec focused on behaviour, not selectors.

## Success Criteria

- [ ] A Playwright spec exists that navigates to `/books`.
- [ ] The test asserts there is at least one book item.
- [ ] The test passes reliably without manual `sleep` calls.
- [ ] (Optional) A `BooksPage` POM is used from the test.
