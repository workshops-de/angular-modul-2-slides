Load environment-specific configuration (e.g. API endpoints) at runtime from a JSON file, so the same build can run in different stages (dev, staging, prod) without recompiling.

- **[Create configuration file]** Add `/assets/configuration/api-endpoints.json` with at least a `books` endpoint.
  ```json
  { "apiEndpoints": { "books": "http://localhost:4730/books" } }
  ```

---

- **[Add RuntimeConfiguration service]** Create `runtime-configuration/runtime-configuration.ts` with a signal for API endpoints.
  ```ts
  @Injectable({ providedIn: 'root' })
  export class RuntimeConfiguration {
    apiEndpoints = signal<{ books: string }>({ books: '' });
  }
  ```

---

- **[Implement provideRuntimeConfiguration]** Create `provide-runtime-configuration.ts` that uses `provideAppInitializer` to fetch the JSON via `HttpClient` before bootstrap and populate the signal.
  ```ts
  export function provideRuntimeConfiguration() {
    return provideAppInitializer(async () => {
      const http = inject(HttpClient);
      const runtimeConfiguration = inject(RuntimeConfiguration);
      const configuration = await firstValueFrom(
        http.get<{ apiEndpoints: { books: string } }>('/assets/configuration/api-endpoints.json')
      );
      runtimeConfiguration.apiEndpoints.set(configuration.apiEndpoints);
    });
  }
  ```

---

- **[Register in app config]** Add `provideRuntimeConfiguration()` to the providers in `app.config.ts`. Ensure `provideHttpClient()` is registered so `HttpClient` is available during the initializer.

---

- **[Inject in BookApiClient]** Replace the hardcoded endpoint in `BookApiClient` with values from `RuntimeConfiguration`.
  ```ts
  private readonly runtimeConfiguration = inject(RuntimeConfiguration);
  private readonly endpoint = this.runtimeConfiguration.apiEndpoints().books;
  ```

---

- **[Update ESLint boundaries]** Add a `runtime-configuration` element type and allow shell and book to import it.
