Move the initial `loadBooks()` call from the component into the store using Signal Store lifecycle hooks. The store loads data when it is first used; the component no longer implements `OnInit` or calls `loadBooks`.

- **Add `withHooks` to the store** Import `withHooks` from `@ngrx/signals` and add it to the store configuration. Implement the `onInit` hook to call `store.loadBooks()`.

  ```ts
  import { withHooks } from '@ngrx/signals';

  // ... in signalStore():
  withHooks({
    onInit(store) {
      store.loadBooks();
    },
  });
  ```

---

- **Remove component initialization** In `book-list.component.ts`, remove the `OnInit` interface, the `ngOnInit()` method, and the manual `store.loadBooks()` call. The store will load data automatically when it is injected and first used.
