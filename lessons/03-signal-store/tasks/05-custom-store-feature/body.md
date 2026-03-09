Extract the Books store logic into a reusable custom feature using `signalStoreFeature`. The main store file becomes a thin wrapper; the plugin holds all state, computed, methods, and hooks.

- **Create the plugin file** Add `src/app/books/state/plugins/with-books.ts`. Import `signalStoreFeature` from `@ngrx/signals` and define a `withBooks()` function that returns the result of `signalStoreFeature`.

  ```ts
  import { signalStoreFeature } from '@ngrx/signals';

  export function withBooks() {
    return signalStoreFeature();
    // ... move all store logic here
  }
  ```

---

- **Move store logic into the plugin** Inside `withBooks()`, move the full store configuration: `withState`, `withComputed`, `withMethods`, and `withHooks`. Transfer all related imports (e.g. `Book`, `BookApiClient`, `patchState`, `rxMethod`, `computed`, etc.) into the plugin file. Export `withBooks`.

---

- **Simplify the main store** In `book-store.ts`, remove the moved logic and reduce it to `signalStore(withBooks())`. Keep only the imports required for the main store. Ensure the public API (signals and methods) remains unchanged so existing components continue to work.
