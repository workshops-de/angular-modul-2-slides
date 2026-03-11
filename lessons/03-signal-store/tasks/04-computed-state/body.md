Add a computed `currentBook` to the `BookStore` that derives the current book from the route param, and refactor `book-detail` to use it. The store owns `actualBookDetailISBN`; the component syncs the route `id` into the store via an `effect`.

- **Extend store state and add computed** In `book-store.ts`, add `actualBookDetailISBN: string | null` to `BookStoreState` (initial: `null`). Import `withComputed` from `@ngrx/signals` and `computed` from `@angular/core`. Add `withComputed` with a `currentBook` signal that finds the book whose `id` equals `store.actualBookDetailISBN()`.
  ```ts
  withComputed(store => ({
    currentBook: computed(() =>
      store.books().find(book => book.id === store.actualBookDetailISBN()) ?? undefined
    ),
  })),
  ```

---

- **Add `setCurrentBookISBN` method** In `withMethods`, add `setCurrentBookISBN: (bookISBN: string | null) => patchState(store, { actualBookDetailISBN: bookISBN })`.

---

- **Refactor `book-detail` to use computed state** In `book-detail.component.ts`, use `input('id')` for the route param. Expose `book = this.store.currentBook` and `isLoading = this.store.isLoading` from the injected store. Add an `effect` in the constructor that calls `this.store.setCurrentBookISBN(this.id())` when `id` changes, so the store stays in sync with the route.
