Extend the `BookStore` with RxJS-based loading: add `isLoading` state, implement a `loadBooks` method with `rxMethod`, and call it from `book-list`. Use the existing `BookApiClient` (not an HTTP resource) for fetching data.

- **Extend store state** Add `isLoading: boolean` to `BookStoreState` and set it to `false` in the initial state.
  ```ts
  type BookStoreState = { books: Book[]; isLoading: boolean };
  const initialState = { books: [], isLoading: false };
  ```

---

- **Add `loadBooks` method with `rxMethod`** Import `withMethods`, `rxMethod`, and `patchState` from `@ngrx/signals`. Import `pipe`, `switchMap`, and `tap` (or use `tapResponse` from `@ngrx/operators` for API calls). Extend the store with `withMethods` and implement `loadBooks` that accepts `{ pageSize: number; searchTerm: string }`, manages `isLoading`, and fetches via `BookApiClient.getBooks()`.
  - Set `isLoading: true` when the request starts
  - Use `switchMap` to call the API
  - On success or error, set `isLoading: false` and update `books` on success

---

- **Trigger loading from `book-list`** Implement `OnInit` in `book-list.component.ts`, inject the store, and call `store.loadBooks({ pageSize, searchTerm })` in `ngOnInit()`. Replace any `booksResource.isLoading()` usage with the store’s `isLoading()` signal in the template.
