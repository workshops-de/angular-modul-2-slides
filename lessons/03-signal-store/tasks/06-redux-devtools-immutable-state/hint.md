## withDevtools

`withDevtools` connects the Signal Store to Redux DevTools so you can inspect state and actions:

```ts
import { withDevtools } from '@angular-architects/ngrx-toolkit';

export const BookStore = signalStore({ providedIn: 'root' }, withBooks(), withDevtools('books'));
```

The extensions are activated when you open DevTools and select the "NgRx Signal Store" tab.

See [NgRx Toolkit — withDevtools](https://ngrx-toolkit.angulararchitects.io/docs/with-devtools).

## withImmutableState

`withImmutableState` replaces `withState` and blocks direct mutations:

```ts
import { withImmutableState } from '@angular-architects/ngrx-toolkit';

// In withBooks(), replace withState(initialState) with:
withImmutableState(initialState),
```

Mutations via `patchState`, template bindings, or direct object changes will throw a runtime error in development.

See [NgRx Toolkit — withImmutableState](https://ngrx-toolkit.angulararchitects.io/docs/with-immutable-state).
