---
layout: section
---

# Signal Store

State Management with NgRx Signals

---
layout: why
---

# Why Signal Store?

Components need shared, reactive state. Without a clear pattern, data and logic scatter across services and components — hard to test and debug.

- Scattered state across components and services
- Unclear data flow and side-effect handling
- Difficult to inspect and time-travel through changes
- No shared conventions for async operations

---
layout: little-what
---

# What is NgRx Signal Store?

A lightweight, signal-based state management solution. `signalStore` composes state, computed values, methods, and hooks via plugins — no boilerplate, framework-native.

---
layout: sub-section
---

# Setup

---
layout: little-what
---

# signalStore + withState

Create a store with an initial state. `signalStore` accepts feature functions; `withState` registers your state shape and initial values.

---
layout: two-cols-header
---

# Minimal Store Definition

::left::

- Import `signalStore` and `withState` from `@ngrx/signals`
- Define a state type and initial value
- Export the store — inject it in components

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{1|3-5|7-11|13|*}
import { signalStore, withState } from '@ngrx/signals';
import { Book } from './book';

type BookStoreState = {
  books: Book[];
};

const initialState: BookStoreState = {
  books: [],
};

export const BookStore = signalStore(withState(initialState));
```

</WindowMockup>

---
layout: two-cols-header
---

# Using the Store in a Component

::left::

- Inject `BookStore` in the component
- Expose the store or its signals to the template
- Use `bookStore.books()` in `@for` — signals are reactive

::right::

<WindowMockup codeblock title="book-list.component.ts">

```ts{1|4|6|*}
import { Component, inject } from '@angular/core';
import { BookStore } from './state/book-store';

@Component({ /* ... */ })
export class BookListComponent {
  protected readonly bookStore = inject(BookStore);
}
```

</WindowMockup>

---
layout: task
---

# Setup

---
layout: sub-section
---

# State Mutations

---
layout: sub-section
---

# withMethods

---
layout: little-what
---

# withMethods

Registers methods on the store. The callback receives `store` and can inject dependencies. Return an object of methods (sync or via `rxMethod`).

---
layout: two-cols-header
---

# withMethods Example

::left::

- First argument: `store` — access signals and call other methods
- Second+ arguments: inject dependencies (e.g. `client = inject(BookApiClient)`)

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|4|5|6|*}
import { withMethods, patchState } from '@ngrx/signals';
import { inject } from '@angular/core';

withMethods((store, client = inject(BookApiClient)) => ({
  setSearchTerm: (term: string) =>
    patchState(store, { searchTerm: term }),
})),
```

</WindowMockup>

---
layout: sub-section
---

# patchState

---
layout: little-what
---

# patchState

Updates store state immutably. Pass the `store` and a partial state object — only listed properties are merged.

---
layout: two-cols-header
---

# patchState Example

::left::

- **Object variant**: Merges partial object into current state — use for loading flags, result data, errors
- **Arrow-function variant**: `(state) => newState` — use when the update depends on current state (e.g. append to array, toggle)

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|3|4|5|7|*}
import { patchState } from '@ngrx/signals';

patchState(store, { isLoading: true });
patchState(store, { books, isLoading: false });
patchState(store, { error: 'Failed to load' });

patchState(store, state => ({ ...state, count: state.count + 1 }));
```

</WindowMockup>

---
layout: sub-section
---

# rxMethod

---
layout: little-what
---

# rxMethod

Connects RxJS streams to the store. Accepts an Observable pipeline; use `tap`, `switchMap`, and `tapResponse` to update state when data arrives.

---
layout: two-cols-header
---

# loadBooks with rxMethod and tapResponse

::left::

- `rxMethod` receives a `pipe` with `tap`, `switchMap`, `tapResponse`
- Set `isLoading` at start; update `books` and clear loading on success/error
- `tapResponse` from `@ngrx/operators` maps success and error cleanly

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|9|10|12|13|13-15|*}
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

</WindowMockup>

---
layout: two-cols-header
---

# Call methods from the Component

::left::

Store methods can be easily called on the injected Store-Service.

::right::

<WindowMockup codeblock title="book-list.component.ts">

```ts
import { Component, inject, OnInit } from '@angular/core';
import { BookStore } from './state/book-store';

@Component({
  /* ... */
})
export class BookListComponent implements OnInit {
  protected readonly store = inject(BookStore);

  ngOnInit() {
    this.store.loadBooks();
  }
}
```

</WindowMockup>

---
layout: task
---

# RxJS Streams

---
layout: sub-section
---

# Lifecycle Hooks

---
layout: why
---

# Why Lifecycle Hooks?

Initial data loading should live in the store, not the component. Avoid `ngOnInit` and manual `loadBooks()` calls — the store loads when first used.

---
layout: little-what
---

# withHooks and onInit

`withHooks` provides `onInit`, which runs when the store is first created (e.g. first injection). Use it to trigger initial loading.

---
layout: two-cols-header
---

# Adding onInit to the Store

::left::

- Import `withHooks` from `@ngrx/signals`
- Add `withHooks` after `withMethods`
- Call `store.loadBooks()` in `onInit` — no args if defaults are fine

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|1|4-5|*}
import { withHooks } from '@ngrx/signals';

// Add to signalStore() after withMethods:
withHooks({
  onInit(store) {
    store.loadBooks();
  },
}),
```

</WindowMockup>

---
layout: two-cols-header
---

# onDestroy Hook

::left::

`onDestroy` runs when the store is destroyed — lifecycle follows the injector. Use it for cleanup: clear intervals, cancel subscriptions, release resources.

- Runs **outside injection context** — cannot use `inject()` inside `onDestroy`; inject dependencies in the outer scope
- Component-level store → destroys with the component; root store → survives the app

[Lifecycle Hooks — NgRx](https://ngrx.io/guide/signals/signal-store/lifecycle-hooks)

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|1|5-7|8-10|*}
withHooks((store) => {
  const interval = inject(IntervalService);

  return {
    onInit(store) {
      interval.start();
    },
    onDestroy() {
      interval.clearAll(); // cleanup — no inject() here
    },
  };
});
```

</WindowMockup>

---
layout: task
---

# Lifecycle Hooks

---
layout: sub-section
---

# Computed State

---
layout: why
---

# Why Computed State?

Derived values (e.g. “current book by route param”) belong in the store. Components should not recompute them — the store owns the single source of truth.

---
layout: little-what
---

# withComputed and computed

`withComputed` defines signals derived from existing store signals. Use Angular's `computed` to react to `books()` and `currentBookId()`.

---
layout: two-cols-header
---

# bookByIdParam: Derived Book from Route

::left::

- Add `currentBookId` to state
- `withComputed` returns a `bookByIdParam` signal
- `computed` finds the book whose `id` equals `currentBookId()`

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{*|1-3|5-9|11|*}
import { withComputed } from '@ngrx/signals';
import { computed } from '@angular/core';

withComputed(store => ({
  bookByIdParam: computed(() =>
    store.books().find(book => book.id === store.currentBookId()) ?? undefined
  ),
})),
```

</WindowMockup>

---
layout: two-cols-header
---

# Syncing Route Param in the Component

::left::

- Use `input('id')` for the route param
- Expose `book = this.store.bookByIdParam`
- In an `effect`, call `store.setCurrentBookId(this.id())` when `id` changes — the store stays in sync

::right::

<WindowMockup codeblock title="book-detail.component.ts">

```ts{*|1|3|5-7|*}
id = input('id');
protected book = this.store.bookByIdParam;

constructor() {
  effect(() => this.store.setCurrentBookId(this.id()));
}
```

</WindowMockup>

---
layout: task
---

# Computed State

---
layout: sub-section
---

# Custom Store Feature

---
layout: why
---

# Why signalStoreFeature?

Reuse store logic across features or apps. Extract state, computed, methods, and hooks into a plugin; the main store becomes a thin wrapper.

---
layout: little-what
---

# signalStoreFeature

`signalStoreFeature` wraps `withState`, `withComputed`, `withMethods`, and `withHooks` into a reusable feature function. The main store composes plugins.

---
layout: two-cols-header
---

# Extracting withBooks Plugin

::left::

- Create `withBooks()` that returns `signalStoreFeature(...)`
- Move all store logic into the plugin
- Import `BookApiClient`, `patchState`, `rxMethod`, `computed`, etc. in the plugin file

::right::

<WindowMockup codeblock title="plugins/with-books.ts">

```ts{*|1|2|3-6|*}
export function withBooks() {
  return signalStoreFeature(
    withState(initialState),
    withComputed(store => ({ ... })),
    withMethods((store, client = inject(BookApiClient)) => ({ ... })),
    withHooks({ onInit(store) { ... } })
  );
}
```

</WindowMockup>

---
layout: two-cols-header
---

# Thin Main Store

::left::

- The main store only composes `withBooks()`
- Public API (signals and methods) stays the same
- Components require no changes

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{1-4|6|*}
import { signalStore } from '@ngrx/signals';
import { withBooks } from './plugins/with-books';

export const BookStore = signalStore(
  { providedIn: 'root' },
  withBooks()
);
```

</WindowMockup>

---
layout: task
---

# Custom Store Feature

---
layout: sub-section
---

# Redux DevTools & Immutable State

---
layout: why
---

# Why DevTools and Immutability?

Inspect state, actions, and time-travel in the browser. Protect against accidental mutations — immutable updates prevent hard-to-debug side effects.

---
layout: little-what
---

# withDevtools and withImmutableState

`withDevtools` from `@angular-architects/ngrx-toolkit` connects the store to Redux DevTools. `withImmutableState` replaces `withState` and blocks direct mutations.

---
layout: two-cols-header
---

# Redux DevTools

::left::

- Add `withDevtools('books')` to the store
- Open Redux DevTools in the browser
- Inspect state, actions, and time-travel through changes

::right::

<WindowMockup codeblock title="book-store.ts">

```ts{1|4|6|*}
import { withDevtools } from '@angular-architects/ngrx-toolkit';

export const BookStore = signalStore(
  { providedIn: 'root' },
  withBooks(),
  withDevtools('books')
);
```

</WindowMockup>

---
layout: two-cols-header
---

# Immutable State

::left::

- Replace `withState` with `withImmutableState` in the plugin
- Direct mutations throw a runtime error
- `patchState` and template bindings are safe

::right::

<WindowMockup codeblock title="plugins/with-books.ts">

```ts{1|4-5|7|*}
import { withImmutableState } from '@angular-architects/ngrx-toolkit';

// In withBooks(), replace withState(initialState) with:
withImmutableState(initialState),
```

</WindowMockup>

---
layout: task
---

# Redux DevTools & Immutable State

---
layout: sub-section
---

# Events (Redux Architecture)

---
layout: why
---

# Why Events?

Redux-style: view dispatches events, store updates state and runs side effects. Predictable flow — events are the single source of intent; reducers and handlers stay pure.

---
layout: little-what
---

# eventGroup, withReducer, withEventHandlers

Define events with `eventGroup` and `type`. Use `withReducer` to map events to state updates; `withEventHandlers` to listen, call APIs, and dispatch success/failure events.

---
layout: two-cols-header
---

# Defining Events

::left::

- Import `eventGroup` and `type`
- Events: `queryChanged`, `loadedSuccess`, `loadedFailure`
- Typed payloads for type-safe dispatching

::right::

<WindowMockup codeblock title="book-search.events.ts">

```ts{1-5|7-11|13|*}
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

</WindowMockup>

---
layout: two-cols-header
---

# withReducer and on

::left::

- **withReducer**: Registers pure reducers that map events to state updates
- **on(event, updater)**: When the event is dispatched, the updater receives the event and returns the new state slice

::right::

<WindowMockup codeblock title="book-search.store.ts">

```ts{*|1|2|*}
withReducer(
  on(queryChanged, () => ({ isLoading: true })),
  on(loadedSuccess, ({ payload: books }) => ({ books, isLoading: false })),
  on(loadedFailure, () => ({ isLoading: false }))
);
```

</WindowMockup>

---
layout: two-cols-header
---

# withEventHandlers

::left::

- **withEventHandlers**: Listens to events, runs side effects (e.g. API calls), and dispatches result events
- Use `events.on(event)` to subscribe
- Handlers return an Observable with an Event

::right::

<WindowMockup codeblock title="book-search.store.ts">

```ts{*|4|5|6|7|9-11|*}
import { Events, withEventHandlers } from '@ngrx/signals/events';
import { mapResponse } from '@ngrx/operators';

withEventHandlers((store, events = inject(Events), ...) => ({
  loadBooks$: events.on(queryChanged).pipe(
    debounceTime(300),
    switchMap(({ payload }) =>
      client.getBooks(10, payload.query).pipe(
        mapResponse({
          next: books => loadedSuccess(books),
          error: err => loadedFailure(err.message),
        })
      )
    ),
  ),
})),
```

</WindowMockup>

---
layout: two-cols-header
---

# Dispatching from the Component

::left::

- Use `injectDispatch` to get the dispatcher
- On input or debounced control: `dispatch.queryChanged({ query })`

::right::

<WindowMockup codeblock title="book-search.component.ts">

```ts{*|3|3,6|*}
import { injectDispatch } from '@ngrx/signals/events';

private readonly dispatch = injectDispatch(bookSearchEvents);

onSearchInput(query: string) {
  this.dispatch.queryChanged({ query });
}
```

</WindowMockup>

---
layout: task
---

# Events (Redux Architecture)
