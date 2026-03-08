## Hints

<details>
<summary>💡 Pattern for shell</summary>

The shell pattern `./src/app/*app*` matches `app.component.ts`, `app.config.ts`, `app.routes.ts`. Adjust if your ESLint config uses a different structure or `overrides` per file pattern.

</details>

<details>
<summary>💡 ESLint overrides</summary>

If your project uses `overrides` in `.eslintrc.json`, add the `settings` and `boundaries` rules to the override that applies to your `src/**/*.ts` files.

</details>
