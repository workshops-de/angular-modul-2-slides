## Overview

In this task, you will set up your development environment and get familiar with the project structure.

## Prerequisites

- Node.js 18+ installed
- Code editor (VS Code recommended)
- Git installed

## Steps

### 1. Clone the Repository

```bash
git clone https://github.com/workshops-de/example-repo.git
cd example-repo
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Start the Development Server

```bash
npm run dev
```

### 4. Verify Setup

Open your browser and navigate to `http://localhost:3000`. You should see the welcome page.

## Code Examples

Here are some examples showing Shiki syntax highlighting features:

### TypeScript with Line Highlighting

```typescript
interface User {
  id: number; // [!code highlight]
  name: string;
  email: string;
  isActive: boolean; // [!code highlight]
  createdAt: Date; // [!code highlight]
  updatedAt: Date; // [!code highlight]
}
```

### Diff Highlighting

```typescript
function greet(name: string) {
  console.log('Hello'); // [!code --]
  console.log(`Hello, ${name}!`); // [!code ++]
}
```

### Focus with Blur

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root', // [!code focus]
  template: '<h1>Hello World</h1>',
})
export class AppComponent {}
```

### Error & Warning Annotations

```typescript
const config = {
  apiUrl: process.env.API_URL, // [!code warning]
  secret: 'hardcoded-secret', // [!code error]
  timeout: 5000,
};
```

## Success Criteria

- [ ] Repository cloned successfully
- [ ] All dependencies installed without errors
- [ ] Development server running
- [ ] Welcome page visible in browser

## Troubleshooting

If you encounter issues:

- Make sure you're using the correct Node.js version (check `.nvmrc`)
- Clear `node_modules` and reinstall: `rm -rf node_modules && npm install`
- Check the console for error messages
