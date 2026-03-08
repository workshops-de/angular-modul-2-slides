<details>
<summary>💡 Hint 1: Feature folder naming</summary>

Use domain names that match your application: `users`, `orders`, `products`, `dashboard`. Avoid technical names like `components` or `features` as top-level folders. Each feature should be self-contained (components, services, models).
</details>

<details>
<summary>💡 Hint 2: public-api.ts structure</summary>

Export only what other features need. Keep internal types and helpers private. Re-export components, services, and interfaces. Use `export * from './file'` or explicit named exports.
</details>

<details>
<summary>💡 Hint 3: eslint-plugin-boundaries configuration</summary>

Define `boundaries/elements` with `type` and `pattern` (e.g. `users` → `src/app/users/*`). Use `boundaries/element-types` rule with `from`/`allow`/`disallow` to define which elements can import from which. Start simple: disallow cross-feature imports.
</details>

<details>
<summary>💡 Hint 4: Facade pattern</summary>

The Facade is an injectable service. Components inject the Facade; the Facade injects the underlying Service or Store. The Facade exposes observables and methods. Avoid exposing raw services to components.
</details>
