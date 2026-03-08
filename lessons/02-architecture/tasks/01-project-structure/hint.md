<details>
<summary>💡 Path alias configuration</summary>

Add a `paths` entry in `tsconfig.json` under `compilerOptions`:

```json
"paths": {
  "@common-components": ["./src/app/common-components/index.ts"]
}
```

Create `common-components/index.ts` that re-exports `MainNavigationComponent` if needed.

</details>

<details>
<summary>💡 Updating imports after moving files</summary>

Use your IDE's "Find and Replace" or "Move" refactoring to update imports. The feature components (BookNew, BookEdit, BookDetail, BookList) should import `BookApiClient` and models from `../../../data/book-api-client` and `../../../data/models` respectively.

</details>
