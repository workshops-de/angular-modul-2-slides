---
layout: cover
---

# Playwright – independent & reliable UI testing

Fast, reliable, developer‑friendly end‑to‑end UI tests across browsers.

---
layout: section
---

# Playwright – independent & reliable UI testing

---
layout: why
---

# Why Playwright?

- Fast feedback through parallel runs and smart isolation.
- Reliable tests thanks to auto‑waiting and web‑aware assertions.
- Developer‑friendly workflow with great IDE integration and codegen.
- Multi‑language and multi‑browser support for real‑world coverage.

---
layout: little-what
---

# What is Playwright?

Playwright is a modern, browser‑independent UI test runner that drives real browsers and web APIs so you can write fast, reliable end‑to‑end tests for your applications.

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Core Playwright test flow

::left::

- Import `test` and `expect` from `@playwright/test`.
- Use the injected `page` fixture to control the browser.
- Follow the Arrange → Act → Assert pattern.

::right::

```ts {all}
import { test, expect } from '@playwright/test';

test('books list has at least one book', async ({ page }) => {
  await page.goto('/books');

  const books = page.getByTestId('book-item');
  await expect(books).toHaveCount(1);
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# First test: Books list

::left::

1. Start Playwright Test via CLI or VS Code Test Explorer.
2. Navigate to `/books`.
3. Locate book items using a role or test id.
4. Assert that there is at least one item in the list.

::right::

```ts {all}
import { test, expect } from '@playwright/test';

test('shows at least one book in the list', async ({ page }) => {
  await page.goto('/books');

  const bookItems = page.getByRole('listitem', { name: /book/i });
  await expect(bookItems).not.toHaveCount(0);
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Generating tests with Codegen

::left::

- Run `npx playwright codegen http://localhost:4200/books`.
- Record typical user interactions (e.g. search for a book).
- Copy the generated test into your suite.
- Refine locators to use roles, text, or test ids.

::right::

```bash
npx playwright codegen http://localhost:4200/books
```

```ts
test('search for a book', async ({ page }) => {
  await page.goto('http://localhost:4200/books');
  await page.getByPlaceholder('Search books').fill('Clean Code');
  await page.getByRole('button', { name: 'Search' }).click();
  await expect(page.getByText('Clean Code')).toBeVisible();
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Assertions & auto‑waiting

::left::

- Web‑aware matchers: `toBeVisible`, `toHaveText`, `toHaveURL`, `toHaveCount`, …
- Expectations are retried until they pass or time out.
- Auto‑waiting waits for elements to be attached, visible, enabled, and stable.
- You usually do **not** need `sleep` when using Playwright.

::right::

```ts {all}
test('filters books by search term', async ({ page }) => {
  await page.goto('/books');

  await page.getByPlaceholder('Search books').fill('Clean Code');
  await page.getByRole('button', { name: 'Search' }).click();

  await expect(page.getByText('Clean Code')).toBeVisible();
  await expect(page).toHaveURL(/\/books\?query=Clean\+Code/);
  await expect(page.getByTestId('book-item')).toHaveCount(1);
});
```

---
layout: default
---

# Stabilising tests

- Prefer robust locators (roles, text, test ids) over brittle CSS selectors.
- Rely on auto‑waiting instead of arbitrary timeouts.
- Keep tests short and focused on a single behaviour.
- Use fixtures or API calls to create predictable test data.

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Annotations & retries

::left::

- Use `test.skip`, `test.fixme`, `test.fail`, `test.slow` to document intent.
- Tag tests to select subsets for local vs. CI runs.
- Configure `retries` in `playwright.config.ts`, often higher in CI.

::right::

```ts
import { test } from '@playwright/test';

test.skip(process.env.SKIP_E2E === '1', 'E2E tests are disabled locally');

test('books list (flaky backend)', async ({ page }) => {
  test.slow();
  await page.goto('/books');
  // ...
});
```

```ts
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  retries: process.env.CI ? 2 : 0,
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Playwright in your architecture

::left::

- Tests drive Page Objects instead of raw locators.
- Page Objects interact with the browser and your web app.
- API fixtures and mocks help control backend behaviour.
- CI runs Playwright in Docker with retries and sharding.

::right::

```mermaid
flowchart LR
  Dev[Developer] --> Tests[Playwright tests]
  Tests --> POM[BooksPage POM]
  POM --> Browser[Real browser]
  Browser --> WebApp[Books web app]
  WebApp --> API[Backend API]
  Tests --> CI[CI pipeline (retries, sharding, Docker)]
  CI --> Report[Test reports & artifacts]
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Page Object Model: BooksPage

::left::

- Encapsulate navigation, locators, and user flows for the books page.
- Reuse the same `BooksPage` across list, search, and create/delete tests.
- Improves readability and maintainability of your test suite.

::right::

```ts {all}
import { expect, Page } from '@playwright/test';

export class BooksPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/books');
  }

  books() {
    return this.page.getByTestId('book-item');
  }

  async search(term: string) {
    await this.page.getByPlaceholder('Search books').fill(term);
    await this.page.getByRole('button', { name: 'Search' }).click();
  }
}
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# API tests & network mocks

::left::

- Seed or clean up data using the `request` fixture.
- Mock unstable backends with `page.route` or HAR recordings.
- Combine fast API checks with full UI journeys.

::right::

```ts {all}
import { test, expect, request } from '@playwright/test';

test('shows API-created book in UI', async ({ page }) => {
  const api = await request.newContext();
  await api.post('/api/books', { data: { title: 'API Book' } });

  await page.goto('/books');
  await expect(page.getByText('API Book')).toBeVisible();
});
```

```ts
await page.route('**/api/books', async (route) => {
  const mock = { items: [{ id: 1, title: 'Mocked Book' }] };
  await route.fulfill({ json: mock });
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Accessibility tests with axe

::left::

- Use `@axe-core/playwright` to run WCAG A/AA checks.
- Catch common accessibility issues early in CI.
- Focus on key flows like the books list and forms.

::right::

```ts {all}
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('books page has no obvious accessibility violations', async ({ page }) => {
  await page.goto('/books');

  const results = await new AxeBuilder({ page }).withTags(['wcag2a', 'wcag2aa']).analyze();

  expect(results.violations).toEqual([]);
});
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Scaling Playwright in CI

::left::

- Configure retries and timeouts differently for CI than for local runs.
- Use sharding to split large test suites across multiple agents.
- Generate HTML reports and traces for failed tests.

::right::

```ts
// playwright.config.ts
export default defineConfig({
  retries: process.env.CI ? 2 : 0,
  reporter: [['html', { open: 'never' }]],
});
```

```bash
npx playwright test --shard=1/3
npx playwright test --shard=2/3
npx playwright test --shard=3/3
```

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Playwright in Docker

::left::

- Use the official `mcr.microsoft.com/playwright` base image.
- Install your app dependencies and run tests inside the container.
- Great for reproducible local runs and CI pipelines.

::right::

```dockerfile
FROM mcr.microsoft.com/playwright:v1.49.0-focal

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

CMD ["npx", "playwright", "test"]
```

---
layout: task
---

# Task - Playwright

---
layout: what-if
---

# What’s next with Playwright?

- Scale up to hundreds of tests with sharding, parallelism, and tagged subsets.
- Use network mocks and HARs when the backend is unstable.
- Extend coverage with more API and accessibility tests.

---
layout: ask-me-anything
---

# Ask me anything – Playwright

Questions about integrating Playwright into your stack, CI/CD setup, or making flaky tests a thing of the past?

---
layout: section
---

# Playwright – independant & reliable UI Testing

Fast, reliable UI tests across all major browsers – with great IDE integration and multi‑language APIs.

---
layout: why
---

# Why Playwright?

Components and flows need to be tested end‑to‑end across real browsers. Without solid tooling, UI tests are slow, flaky and hard to debug.

- Flaky Tests durch manuelle `wait`/`sleep`‑Aufrufe
- Langsame Feedback‑Schleifen in CI/CD‑Pipelines
- Schwer nachvollziehbare Fehler in Headless‑Browsern
- Schlechte IDE‑Integration und Debugging‑Erfahrung

Playwright adressiert diese Punkte mit:

- **Schnellen, zuverlässigen Tests** über Chromium, Firefox und WebKit
- **Starker IDE‑Integration** (v.a. VS Code) inkl. Test Explorer & Debugging
- **Multi‑Language‑APIs** für **Node.js, Python, .NET und Java**
- Integrierten Best Practices (Auto‑Waiting, Assertions, POM, API‑Tests, Mocks, A11y, Retries, Sharding, Docker)

---
layout: little-what
---

# What is Playwright?

Playwright ist ein modernes End‑to‑End‑Testing‑Framework für schnelle, zuverlässige UI‑Tests über alle gängigen Browser hinweg – mit erstklassiger IDE‑Integration, mächtigen Debug‑Tools, Multi‑Language‑APIs (Node.js, Python, .NET, Java) und Unterstützung für etablierte Test‑Patterns wie Page Object Models, API‑Tests und Accessibility‑Checks.

---
layout: sub-section
---

# Core Testing Flow mit Playwright

---
layout: two-cols-header
---

# Erste Tests schreiben

::left::

- **Ziel:** Ein erster stabiler UI‑Test für die Bücher‑Seite der Workshop‑App
- Bausteine:
  - `test(...)` und `expect(...)` aus `@playwright/test`
  - Das `page`‑Fixture für Browser‑Interaktionen
  - Test‑Isolation: jeder Test bekommt einen eigenen Browser‑Kontext
- Typischer Ablauf:
  - Dev‑Server starten (`ng serve` o.ä.)
  - `npx playwright test` ausführen
  - Reporter/Fehlermeldungen lesen

::right::

<WindowMockup codeblock title="tests/books-basic.spec.ts">

```ts{*|5-9|11-16|*}
import { test, expect } from '@playwright/test';

test('zeigt mindestens ein Buch an', async ({ page }) => {
  await page.goto('http://localhost:4200/books');

  const books = page.getByTestId('book-item');

  // einfache, aber echte Assertion
  await expect(books).not.toHaveCount(0);
});
```

</WindowMockup>

---
layout: two-cols-header
---

# Tests generieren mit Codegen

::left::

- **Motivation**
  - Schneller Einstieg in Locators und typische Flows
  - Gute Basis, die später in POMs und cleanen Tests endet
- **Workflow**
  - `npx playwright codegen http://localhost:4200/books`
  - Im Browser klicken, im Inspector Assertions hinzufügen
  - Code kopieren, in eigenes Testfile übernehmen und aufräumen
- **Best Practice**
  - Codegen ist Startpunkt, nicht Endzustand
  - Eindeutige Locators (`getByRole`, `getByTestId`, Text) bevorzugen

::right::

<WindowMockup codeblock title="tests/books-codegen.spec.ts">

```ts{*|3-8|10-14|*}
import { test, expect } from '@playwright/test';

test('öffnet Bücherliste (Codegen)', async ({ page }) => {
  await page.goto('http://localhost:4200/');
  await page.getByRole('link', { name: 'Bücher' }).click();

  await expect(
    page.getByRole('heading', { name: 'Bücher' })
  ).toBeVisible();
});
```

</WindowMockup>

---
layout: two-cols-header
---

# Assertions & Auto‑Waiting

::left::

- **expect‑API**
  - Generische Matcher: `toEqual`, `toContain`, `toBeTruthy`, …
  - Web‑spezifische Assertions: `toBeVisible`, `toHaveText`, `toHaveURL`, `toHaveCount`, …
- **Auto‑Waiting**
  - Vor Aktionen prüft Playwright: Element ist sichtbar, stabil, klickbar
  - Assertions werden automatisch wiederholt, bis die Bedingung erfüllt ist (oder Timeout)
- **Vorteil**
  - Kaum noch manuelle `wait`/`sleep`‑Aufrufe
  - Deutlich weniger flaky Tests

::right::

<WindowMockup codeblock title="tests/books-search.spec.ts">

```ts{*|5-8|10-17|*}
import { test, expect } from '@playwright/test';

test('Suchergebnis für ein Buch', async ({ page }) => {
  await page.goto('http://localhost:4200/books');

  await page.getByPlaceholder('Suche').fill('Clean Code');
  await page.getByRole('button', { name: 'Suchen' }).click();

  const result = page
    .getByTestId('book-item')
    .filter({ hasText: 'Clean Code' });

  await expect(result).toHaveCount(1);
  await expect(result).toBeVisible();
});
```

</WindowMockup>

---
layout: sub-section
---

# Tests in der Praxis stabilisieren

---
layout: two-cols
---

# Annotations & Retries

::left::

- **Annotations**
  - `test.skip(...)`: Test überspringen (z.B. nicht relevant auf Mobile)
  - `test.fail(...)`: Erwarteter Fehler (z.B. dokumentierte Regression)
  - `test.fixme(...)`: Defekter Test, wird nicht ausgeführt
  - `test.slow(...)`: Timeout für teure Tests erhöhen
- **Retries**
  - Global oder pro `describe` konfigurierbar
  - Tests werden als _passed_, _flaky_ oder _failed_ klassifiziert
- **Tags**
  - Titel mit `@slow`, `@a11y` usw. versehen und über `--grep` filtern

::right::

<WindowMockup codeblock title="tests/books-retries.spec.ts">

```ts{*|3-5|7-15|*}
import { test, expect } from '@playwright/test';

test.describe('Books (kritische Flows)', () => {
  test.describe.configure({ retries: 2 });

  test('Suche – bekannter Bug in Firefox', async ({ page, browserName }) => {
    test.skip(browserName === 'firefox', 'Bug #1234 in Firefox noch offen');

    await page.goto('http://localhost:4200/books');
    // ...
    await expect(page.getByTestId('book-item')).not.toHaveCount(0);
  });
});
```

</WindowMockup>

---
layout: image-right
---

# Debugging mit VS Code & Playwright

::left::

- **Playwright‑Extension für VS Code**
  - Test Explorer mit „Run Test“ und **„Debug Test“**
  - Breakpoints setzen, Variablen inspizieren, Call‑Stack ansehen
- **Typischer Debug‑Flow**
  - Test im Explorer → Rechtsklick → „Debug Test“
  - Browser öffnet sich; Breakpoint stoppt an der gewünschten Zeile
  - Fehlermeldungen im Editor + Detailansicht im Playwright‑Panel
- **Screenshots, die du hier zeigen kannst**
  - Debugger mit gesetztem Breakpoint
  - Test Error‑Ansicht mit Locator‑Details
  - Kontextmenü „Start debugging“ im Test Explorer

::right::

> Platzhalter für drei Debugging‑Screenshots (IDE + Test Explorer + Fehlermeldung).

---
layout: sub-section
---

# Architektur & Integration

---
layout: two-cols-header
---

# Page Object Model (POM)

::left::

- Jede Seite / jeder Screen erhält eine eigene POM‑Klasse
- Selektoren und typische Aktionen liegen im POM, nicht im Test
- Tests lesen sich wie Fach‑Stories

::right::

<WindowMockup codeblock title="tests/pom/BooksPage.ts">

```ts{*|5-10|12-18|20-25|*}
import { type Page, expect } from '@playwright/test';

export class BooksPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('http://localhost:4200/books');
  }

  get books() {
    return this.page.getByTestId('book-item');
  }

  async search(term: string) {
    await this.page.getByPlaceholder('Suche').fill(term);
    await this.page.getByRole('button', { name: 'Suchen' }).click();
  }

  async expectAtLeastOneBook() {
    await expect(this.books).not.toHaveCount(0);
  }
}
```

</WindowMockup>

---
layout: two-cols-header
---

# Tests mit POM schreiben

::left::

- Tests verwenden POM‑Methoden anstatt rohe `page`‑Aufrufe
- Änderungen in der UI → nur POM anpassen
- Tests bleiben kurz und lesbar

::right::

<WindowMockup codeblock title="tests/books-pom.spec.ts">

```ts{*|3|5-11|*}
import { test } from '@playwright/test';
import { BooksPage } from './pom/BooksPage';

test('mindestens ein Buch – mit POM', async ({ page }) => {
  const booksPage = new BooksPage(page);

  await booksPage.goto();
  await booksPage.expectAtLeastOneBook();
});
```

</WindowMockup>

---
layout: two-cols-header
---

# API‑Tests & Mocks

::left::

- **API‑Tests**
  - Nutzen das `request`‑Fixture, ohne Browser
  - Eignen sich für Setup/Teardown von Testdaten
- **Mocks**
  - `page.route(...)` zum Mocken von Endpunkten
  - HAR‑Replays für vollständige aufgezeichnete Szenarien
- Kombination: Zustand per API vorbereiten, UI mit Playwright prüfen

::right::

<WindowMockup codeblock title="tests/books-api.spec.ts">

```ts{*|5-10|12-18|*}
import { test, expect } from '@playwright/test';

test('legt Buch per API an und prüft UI', async ({ page, request }) => {
  const response = await request.post('/api/books', {
    data: { title: 'Clean Code', author: 'Robert C. Martin' },
  });
  expect(response.ok()).toBeTruthy();

  await page.goto('http://localhost:4200/books');

  await expect(
    page.getByTestId('book-item').filter({ hasText: 'Clean Code' })
  ).toHaveCount(1);
});
```

</WindowMockup>

---
layout: two-cols-header
---

# Accessibility‑Tests mit axe

::left::

- Viele A11y‑Probleme sind automatisiert erkennbar:
  - Fehlende Labels, schlechte Kontraste, kaputte ARIA‑Struktur
- `@axe-core/playwright` integriert axe in Playwright‑Tests
- A11y‑Checks lassen sich in bestehende UI‑Tests integrieren

::right::

<WindowMockup codeblock title="tests/books-a11y.spec.ts">

```ts{*|3-4|6-13|*}
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('Books-Seite hat keine offensichtlichen A11y-Probleme', async ({ page }) => {
  await page.goto('http://localhost:4200/books');

  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();

  expect(results.violations).toEqual([]);
});
```

</WindowMockup>

---
layout: sub-section
---

# CI/CD & Skalierung

---
layout: two-cols
---

# Retries & Sharding im CI

::left::

- **Retries**
  - Machen sporadische Infrastruktur‑Fehler robuster
  - Klassifizieren Tests als _passed_, _flaky_ oder _failed_
- **Sharding**
  - Teilt die Test‑Suite per `--shard=x/y` auf mehrere CI‑Jobs auf
  - Hilfreich bei großen E2E‑Suites

::right::

<WindowMockup codeblock title="playwright.config.ts">

```ts{*|3-7|*}
import { defineConfig } from '@playwright/test';

export default defineConfig({
  retries: 2,
  reporter: process.env.CI ? 'blob' : 'list',
});
```

</WindowMockup>

---
layout: two-cols
---

# Playwright in Docker

::left::

- **Motivation**
  - Reproduzierbare Umgebung für Browser + Dependencies
  - Gleiche Runtime lokal und im CI
- **Ansatz**
  - Offizielles Playwright‑Image nutzen
  - Tests im Container ausführen, Reports/Traces herauskopieren

::right::

<WindowMockup codeblock title="Dockerfile">

```{*|1|3-8|10|*}
FROM mcr.microsoft.com/playwright:v1.58.2-noble

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .

CMD ["npx", "playwright", "test"]
```

</WindowMockup>

---
layout: task
---

# Hands-on: Bücher mit Playwright testen

- **1. Ensure at least one book in list**
  - Navigiere zur Bücher‑Seite der Workshop‑App.
  - Schreibe einen Test (gern mit POM), der sicherstellt, dass mindestens ein `book-item` vorhanden ist.
- **2. Search book and verify search result**
  - 2.1 Suche nach einem existierenden Buch (z.B. „Clean Code“) und verifiziere, dass das Suchergebnis dieses Buch (oder den erwarteten Treffer) enthält.
  - 2.2 Leere die Suche (Eingabe löschen oder „Zurücksetzen“) und stelle sicher, dass die Ergebnisliste leer ist bzw. den definierten „Keine Ergebnisse“‑Zustand zeigt.
- **3. Create new book and ensure book in list, delete book afterwards**
  - Lege über die UI ein neues Buch an und verifiziere, dass es in der Liste erscheint.
  - Lösche dieses Buch anschließend wieder und prüfe, dass es nicht mehr angezeigt wird, damit dein Testdaten‑Zustand sauber bleibt.
