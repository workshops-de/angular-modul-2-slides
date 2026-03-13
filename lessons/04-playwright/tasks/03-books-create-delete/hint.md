## Hints

- Use a helper to generate unique titles, for example:

```ts
const title = `Playwright Book ${Date.now()}`;
```

- When selecting the created book for deletion, use a locator that includes the title text so you only delete the right row.
- If the UI shows a confirmation dialog, assert on it and make sure to confirm the deletion.
- Re‑run the test a few times locally to be sure it does not depend on leftover data.
