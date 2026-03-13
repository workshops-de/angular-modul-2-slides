## Hints

- Reuse the navigation and locator logic from the first task.
- Make sure your test data actually contains the book title you search for (e.g. “Clean Code”).
- When checking for “no results”, assert on a **clear signal**:
  - an empty list
  - or a dedicated “No books found” message.
- Use Playwright’s auto‑waiting: assertions like `toBeVisible` and `toHaveCount` will retry until stable.
