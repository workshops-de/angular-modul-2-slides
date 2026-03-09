## Signal Store structure

Use `signalStore` with `withState` from `@ngrx/signals`:

```ts
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

See [NgRx Signals — Install](https://ngrx.io/guide/signals/install) for setup details.

## Injecting and using the store in the component

Inject `BookStore` in `book-list.component.ts` and expose it as a template variable. In the template, use the store's `books` signal in the `@for` loop (e.g. `bookStore.books()`).
