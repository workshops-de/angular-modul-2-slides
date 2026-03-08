Configure ESLint with `eslint-plugin-boundaries` so that imports between domains (shell, book, common-components) follow defined rules and cross-boundary violations are reported.

- **[Install resolver]** Install `eslint-import-resolver-typescript` so that path aliases and TypeScript paths are resolved correctly.
  ```bash
  npm install eslint-import-resolver-typescript --save-dev
  ```

---

- **[Configure import/resolver]** Add `settings` to `.eslintrc.json` so ESLint can resolve path aliases like `@common-components`.
  ```json
  "settings": {
    "import/resolver": {
      "typescript": { "alwaysTryTypes": true }
    }
  }
  ```

---

- **[Define element types]** Add `boundaries/elements` to define domains by file pattern:
  - `book` → `./src/app/book/**/*.ts`
  - `common-components` → `./src/app/common-components/**/*.ts`
  - `shell` → `./src/app/*app*` (app shell files)
  ```json
  "boundaries/elements": [
    { "type": "book", "pattern": "./src/app/book/**/*.ts", "mode": "file" },
    { "type": "common-components", "pattern": "./src/app/common-components/**/*.ts", "mode": "file" },
    { "type": "shell", "pattern": "./src/app/*app*", "mode": "file" }
  ]
  ```

---

- **[Add boundary rules]** Configure `boundaries/element-types` to disallow cross-domain imports except:
  - Shell may import: shell, common-components
  - Book may import: book only
  ```json
  "boundaries/element-types": [2, {
    "default": "disallow",
    "rules": [
      { "from": "shell", "allow": ["shell", "common-components"] },
      { "from": "book", "allow": ["book"] }
    ]
  }],
  "boundaries/no-unknown": [2]
  ```

---

- **[Verify with imports]** Ensure `app.component.ts` uses `@common-components` and that `npm run lint` passes with no boundary violations.
