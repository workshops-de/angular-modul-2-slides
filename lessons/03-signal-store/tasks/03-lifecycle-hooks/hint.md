## withHooks and onInit

`withHooks` provides lifecycle hooks for the store. The `onInit` callback runs when the store is first created (e.g. when first injected). Use it to trigger initial data loading:

```ts
import { withHooks } from '@ngrx/signals';

// Add to signalStore() after withMethods:
withHooks({
  onInit(store) {
    store.loadBooks();
  },
});
```

See [NgRx Signal Store — Lifecycle Hooks](https://ngrx.io/guide/signals/signal-store/lifecycle-hooks).

## Simplifying the component

Remove `implements OnInit`, `ngOnInit()`, and any call to `store.loadBooks()` from the component. The store handles initialization on first use.
