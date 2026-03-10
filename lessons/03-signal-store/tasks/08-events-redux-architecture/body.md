Add a Book Search page using the NgRx Signal Store Events plugin: define events, build a store with reducers and event handlers, and dispatch from the view. Follow a Redux-style flow: the view dispatches events, the store updates state and triggers side effects.

- **Create the Book Search page and route** Add a `book-search` route and component. The page shows a search input and displays books matching the query.

---

- **Define event groups** Create `book-search.events.ts` and define `bookSearchEvents` with `eventGroup` from `@ngrx/signals/events`:
  - `queryChanged`: payload `{ query: string }` — fired when the user types in the search input
  - `loadedSuccess`: payload `Book[]` — API returned books
  - `loadedFailure`: payload `Error` (or `string`) — API failed

---

- **Build BookSearchStore with reducers and event handlers** Create `book-search.store.ts` using `withReducer` and `withEventHandlers`:
  - **Reducers** (`withReducer` + `on`): handle `queryChanged` → set `isLoading: true`; handle `loadedSuccess` → update `books` and set `isLoading: false`; handle `loadedFailure` → set `isLoading: false` and log to console
  - **Event handler** (`withEventHandlers`): listen to `queryChanged`, call `BookApiClient.getBooks()` (or search method), use `mapResponse` from `@ngrx/operators` to dispatch `loadedSuccess` on success and `loadedFailure` on error; in the error path, call `console.error` before dispatching

---

- **Dispatch from the Book Search component** In the search component, inject the store and use `injectDispatch(bookSearchEvents)` to get the dispatcher. On input (e.g. `(input)` or `(search)`) or via a debounced control, dispatch `queryChanged({ query })`. Bind the store’s `books` and `isLoading` signals to the template.
