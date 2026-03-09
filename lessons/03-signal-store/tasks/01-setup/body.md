Set up NgRx Signal Store for the Books feature: install the package, create the store, and wire it into `book-list` so the template uses the store’s signal instead of the old resource.

- **Install `@ngrx/signals`** Add the package via npm.
  ```bash
  npm install @ngrx/signals
  ```

---

- **Create the store file** Add `src/app/books/state/book-store.ts` and define a `BookStoreState` interface with a `books: Book[]` array. Set the initial state to an empty array.
  - Import `signalStore` and `withState` from `@ngrx/signals`
  - Create `BookStore` using `signalStore(withState(initialState))`

---

- **Wire the store into `book-list`** Import and inject `BookStore` in `book-list.component.ts`. In the template, replace the `@for` loop source with the store’s `books` signal (e.g. `books()` from the injected store).

> **Note:** The app will not work fully yet because the store is empty. The next tasks will load data and restore behavior.
