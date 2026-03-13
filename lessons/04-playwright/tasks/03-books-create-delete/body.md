## Overview

In this task, you will write a Playwright test that **creates and deletes** a book via the UI.

## Goal

- Create a new book with a **unique title** through the UI.
- Assert that the new book appears in the books list.
- Delete the same book again and assert that it is no longer present.

## Steps

1. Decide how to create a unique title (e.g. timestamp or random suffix).
2. Navigate to `/books` and open the “Create book” form.
3. Fill in the form fields and submit.
4. Assert that the newly created book appears in the list.
5. Trigger the delete action for that specific book.
6. Assert that the book has been removed from the list.

## Data hygiene

- Make sure your test **cleans up after itself**:
  - Create exactly one book.
  - Delete the same book before the test finishes.
- This keeps repeated runs and CI pipelines clean.

## Success Criteria

- [ ] A test creates a uniquely named book via the UI.
- [ ] The test asserts that the new book appears in the list.
- [ ] The test deletes the same book and verifies it is gone.
- [ ] The test is repeatable and does not leave stale data behind.
