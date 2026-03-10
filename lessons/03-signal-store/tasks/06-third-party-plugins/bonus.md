## withRoute (NgRx Traits)

Install NgRx Traits and use `withRoute` to derive route params, query params, or route data as computed signals in the store:

```bash
npm install @ngrx-traits/signals
```

```ts
import { withRoute } from '@ngrx-traits/signals';
import { withCalls } from '@ngrx-traits/signals';
import { withHooks } from '@ngrx-traits/signals';

const BookStore = signalStore(
  withRoute(({ params, queryParams }) => ({
    id: params['id'] as string,
    page: +(queryParams['page'] ?? 1),
  })),
  withCalls(/* ... */),
  withHooks(({ loadBook, id }) => ({
    onInit: () => loadBook(id()),
  }))
);
```

This removes the need for `inject(ActivatedRoute)` and manual subscriptions in the component. See [withRoute](https://ngrx-traits.dev/docs/traits/with-route).
