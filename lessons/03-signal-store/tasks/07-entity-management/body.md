Refactor the `withBooks` signal store feature to use `withEntities<Book>` from NgRx. Attendees should study the [NgRx Signal Store — Entity Management](https://ngrx.io/guide/signals/signal-store/entity-management) documentation and adopt the solution.

- **Study the docs** Read the entity management guide. Understand `withEntities`, `setEntities`, `removeEntity`, and `selectId`.

---

- **Replace `with(Immutable)State` with `withEntities<Book>`** In `plugins/with-books.ts`, remove the `books: Book[]` state. Import `withEntities` from `@ngrx/signals/entities` and add `withEntities<Book>({ entity: 'book', selectId })` to the store configuration. Define `selectId: (book: Book) => book.isbn` (or the appropriate ID property on your `Book` model).

---

- **Update `loadBooks` with `setEntities`** In `withMethods`, adjust `loadBooks` so it uses `setEntities(books, { selectId })` instead of `patchState(store, { books })`. Import `setEntities` from `@ngrx/signals/entities`.

  ```ts
  import { setEntities } from '@ngrx/signals/entities';

  // In loadBooks rxMethod, on success:
  patchState(store, setEntities(books, { selectId }));
  ```

---

- **Update `removeBook` with `removeEntity`** from `@ngrx/signals/entities`.

---

- **Update templates** Replace any `store.books()` usage with `store.bookEntities()` or the appropriate entity selectors. Ensure list and detail views work with the new entity structure.
