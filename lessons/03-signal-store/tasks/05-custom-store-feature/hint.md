## signalStoreFeature structure

`signalStoreFeature` lets you wrap `withState`, `withComputed`, `withMethods`, and `withHooks` into a reusable feature function:

```ts
import { signalStoreFeature, withState, withComputed, withMethods, withHooks, patchState } from '@ngrx/signals';

export function withBooks() {
  return signalStoreFeature(
    withState(initialState),
    withComputed(store => ({ ... })),
    withMethods((store, client = inject(BookApiClient)) => ({ ... })),
    withHooks({ onInit(store) { ... } })
  );
}
```

## Using the plugin in the main store

The main `book-store.ts` becomes a thin wrapper:

```ts
import { signalStore } from '@ngrx/signals';
import { withBooks } from './plugins/with-books';

export const BookStore = signalStore({ providedIn: 'root' }, withBooks());
```

The store’s public API stays the same; components do not need changes. The plugin file owns all logic and dependencies.

See [NgRx Signal Store — Custom Store Features](https://ngrx.io/guide/signals/signal-store/custom-store-features).
