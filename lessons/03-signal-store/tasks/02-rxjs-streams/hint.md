## loadBooks with rxMethod and tapResponse

Use `withMethods`, `rxMethod`, and `patchState` from `@ngrx/signals`. Install `@ngrx/operators` for `tapResponse`, which is recommended for handling API calls:

```bash
npm install @ngrx/operators
```

```ts
import { signalStore, withState, withMethods, patchState } from '@ngrx/signals';
import { rxMethod } from '@ngrx/signals/rxjs-interop';
import { pipe, tap, switchMap } from 'rxjs';
import { tapResponse } from '@ngrx/operators';
import { inject } from '@angular/core';

// ... in withMethods:
withMethods((store, client = inject(BookApiClient)) => ({
  loadBooks: rxMethod<{ pageSize: number; searchTerm: string }>(
    pipe(
      tap(() => patchState(store, { isLoading: true })),
      switchMap(props => client.getBooks(props.pageSize, props.searchTerm)),
      tapResponse({
        next: books => patchState(store, { books, isLoading: false }),
        error: () => patchState(store, { isLoading: false }),
      })
    )
  ),
})),
```

See [NgRx Signals — RxJS Integration](https://ngrx.io/guide/signals/rxjs-integration) and [NgRx Operators — tapResponse](https://ngrx.io/guide/operators/operators).

## Calling loadBooks from the component

In `ngOnInit()`, call `this.bookStore.loadBooks({ pageSize: 10, searchTerm: '' })` (or with values from your component). Expose the store’s `isLoading()` signal in the template for loading UI.
