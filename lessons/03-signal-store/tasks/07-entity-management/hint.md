## withEntities and entity helpers

Import `withEntities`, `setEntities`, and `removeEntity` from `@ngrx/signals/entities`. Replace `withState` with `withEntities<Book>`:

```ts
import { withEntities, setEntities, removeEntity } from '@ngrx/signals/entities';

const selectId = (book: Book) => book.isbn;

withEntities<Book>(),
```

See [NgRx Signal Store — Entity Management](https://ngrx.io/guide/signals/signal-store/entity-management).

## loadBooks with setEntities

Use `setEntities` in the success callback of `tapResponse`:

```ts
import { setEntities } from '@ngrx/signals/entities';

next: books => patchState(store, setEntities(books, { selectId })),
```

## removeBook with removeEntity

```ts
import { removeEntity } from '@ngrx/signals/entities';

withMethods((store, router = inject(Router), bookApiClient = inject(BookApiClient)) => ({
  // ...
  removeBook: rxMethod<{ bookISBN: string }>(
    pipe(
      switchMap(({ bookISBN }) =>
        bookApiClient.delete(bookISBN).pipe(
          tapResponse({
            next: () => {
              patchState(store, removeEntity(bookISBN));
              router.navigateByUrl('/');
            },
            error: err => console.error(err),
          })
        )
      )
    )
  ),
})),
```

## Using entity signals in the template

After refactoring, access entities via `bookEntities()` and `bookIds()` signals. Use `getEntity` or entity selectors for lookups.
