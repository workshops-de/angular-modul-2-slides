## eventGroup and event types

Define events with `eventGroup` and `type`:

```ts
import { type } from '@ngrx/signals';
import { eventGroup } from '@ngrx/signals/events';

export const bookSearchEvents = eventGroup({
  source: 'Book Search',
  events: {
    queryChanged: type<{ query: string }>(),
    loadedSuccess: type<Book[]>(),
    loadedFailure: type<string>(),
  },
});
```

## withReducer for state transitions

Use `withReducer` and `on` to map events to state updates:

```ts
import { withReducer } from '@ngrx/signals/events';
import { on } from '@ngrx/signals/events';

withReducer(
  on(bookSearchEvents.queryChanged, () => ({ isLoading: true })),
  on(bookSearchEvents.loadedSuccess, ({ payload: books }) => ({ books, isLoading: false })),
  on(bookSearchEvents.loadedFailure, () => ({ isLoading: false }))
);
```

## withEventHandlers and mapResponse

Listen to events, call the API, and map results to success/failure events:

```ts
import { Events, withEventHandlers } from '@ngrx/signals/events';
import { mapResponse } from '@ngrx/operators';
import { inject } from '@angular/core';
import { debounceTime, switchMap } from 'rxjs';

withEventHandlers((store, events = inject(Events), client = inject(BookApiClient)) => ({
  loadBooks$: events.on(bookSearchEvents.queryChanged).pipe(
    debounceTime(300),
    switchMap(({ payload }) =>
      client.getBooks(10, payload.query).pipe(
        mapResponse({
          next: books => bookSearchEvents.loadedSuccess(books),
          error: err => {
            console.error(err);
            return bookSearchEvents.loadedFailure(err.message);
          },
        })
      )
    ),
  ),
})),
```

## Dispatching from the component

Use `injectDispatch` to dispatch events:

```ts
import { injectDispatch } from '@ngrx/signals/events';

private readonly dispatch = injectDispatch(bookSearchEvents);

onSearchInput(query: string) {
  this.dispatch.queryChanged({ query });
}
```

See [NgRx Signal Store — Events](https://ngrx.io/guide/signals/signal-store/events).
