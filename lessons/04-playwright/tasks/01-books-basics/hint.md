## Hints

- Start from the Playwright docs section on **Writing Tests** and the example from the slides.
- If you use `data-testid="book-item"` in the app, prefer `page.getByTestId('book-item')` over long CSS selectors.
- Remember that `expect(locator).toHaveCount(n)` will **auto‑wait** until the DOM is stable.
- When asserting "at least one item", you can use:

```ts
await expect(books).not.toHaveCount(0);
```

- If navigation is flaky, check that the dev server is running and that the base URL is configured correctly in `playwright.config.ts`.
