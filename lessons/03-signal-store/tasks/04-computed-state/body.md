Add a computed `bookByIdParam` to the `BookStore` that derives the current book from the route param, and refactor `book-detail` to use it. The store owns `currentBookId`; the component syncs the route `id` into the store via an `effect`.

- **Extend store state and add computed** In `book-store.ts`, add `currentBookId: string | null` to `BookStoreState` (initial: `null`). Import `withComputed` from `@ngrx/signals` and `computed` from `@angular/core`. Add `withComputed` with a `bookByIdParam` signal that finds the book whose `id` equals `store.currentBookId()`.
  ```ts
  withComputed(store => ({
    bookByIdParam: computed(() =>
      store.books().find(book => book.id === store.currentBookId()) ?? undefined
    ),
  })),
  ```

---

- **Add `setCurrentBookId` method** In `withMethods`, add `setCurrentBookId: (bookId: string | null) => patchState(store, { currentBookId: bookId })`.

---

- **Refactor `book-detail` to use computed state** In `book-detail.component.ts`, use `input('id')` for the route param. Expose `book = this.store.bookByIdParam` and `isLoading = this.store.isLoading` from the injected store. Add an `effect` in the constructor that calls `this.store.setCurrentBookId(this.id())` when `id` changes, so the store stays in sync with the route.
