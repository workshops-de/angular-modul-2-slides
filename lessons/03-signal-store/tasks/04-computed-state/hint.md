## withComputed and bookByIdParam

`withComputed` defines derived state from existing store signals. Use `computed` from Angular to react to `books()` and `currentBookId()`:

```ts
import { withComputed } from '@ngrx/signals';
import { computed } from '@angular/core';

withComputed(store => ({
  bookByIdParam: computed(() =>
    store.books().find(book => book.id === store.currentBookId()) ?? undefined
  ),
})),
```

## Syncing route param in the component

Use an `effect` to write the route `id` into the store whenever it changes. `bookByIdParam` will update reactively:

```ts
id = input('id');
protected book = this.store.bookByIdParam;

constructor() {
  effect(() => this.store.setCurrentBookId(this.id()));
}
```

Ensure `setCurrentBookId` runs when the component is created and when `id` changes. The store remains the single source of truth for the selected book.

See [NgRx Signal Store](https://ngrx.io/guide/signals/signal-store) for store methods and properties.
