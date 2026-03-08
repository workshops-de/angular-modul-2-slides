Reorganize the Book module into a feature-sliced structure so that folders reflect domains and use cases instead of technical layers.

- **[Introduce BookShell]** Replace the flat `BookComponent` with a minimal `BookShell` that acts as a layout container with `<router-outlet>`. The shell owns the route structure and delegates rendering to feature components.
  ```ts
  @Component({
    selector: 'ws-book-shell',
    imports: [RouterOutlet],
    template: '<router-outlet />',
  })
  export class BookShell {}
  ```

---

- **[Create feature folders]** Move book-related components into feature folders: `create-book`, `edit-book`, `view-book-details`, `view-books`. Each feature owns its component(s) and related assets (template, styles).
  - `book-new/` → `features/create-book/book-new/`
  - `book-edit/` → `features/edit-book/book-edit/`
  - `book-detail/` → `features/view-book-details/book-detail/`
  - `book-list/` → `features/view-books/book-list/`

---

- **[Centralize data layer]** Move models and the API client into `book/data/`:
  - `models/` → `data/models/`
  - `book-api.service.ts` → `data/book-api-client.ts`
  - Rename `BookApiService` to `BookApiClient` and update imports in feature components.

---

- **[Update imports and routes]** Adjust `book.routes.ts` to use `BookShell` and the new feature paths. Update all feature components to import from `../../../data/...` instead of relative paths to the old locations.

---

- **[Add path alias]** Configure `@common-components` in `tsconfig.json` so the shell can import shared components without long relative paths.
  ```ts
  import { MainNavigationComponent } from '@common-components';
  ```
