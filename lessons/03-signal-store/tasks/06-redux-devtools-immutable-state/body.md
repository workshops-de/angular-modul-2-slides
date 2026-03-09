Add Redux DevTools support and enforce immutable state using plugins from `@angular-architects/ngrx-toolkit`. You will inspect store state in the browser and protect against accidental mutations.

- **Install the toolkit** Add the NgRx Toolkit package.
  ```bash
  npm install @angular-architects/ngrx-toolkit
  ```

---

- **Add Redux DevTools to the store** In `book-store.ts`, import `withDevtools` from `@angular-architects/ngrx-toolkit` and add it to the store configuration with a name (e.g. `'books'`).

  ```ts
  import { withDevtools } from '@angular-architects/ngrx-toolkit';

  export const BookStore = signalStore({ providedIn: 'root' }, withBooks(), withDevtools('books'));
  ```

---

- **Enforce immutable state in the plugin** In `plugins/with-books.ts`, replace `withState` with `withImmutableState` from `@angular-architects/ngrx-toolkit`. This protects the store from unintended mutations and throws a runtime error if state is mutated.

---

**Install the [Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd)** browser extension to inspect the store state, actions, and time-travel through changes.
