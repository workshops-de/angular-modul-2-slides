## Hints

<details>
<summary>💡 Hint 1: Basic Implementation</summary>

```typescript
process(input: string): string {
  return input.toUpperCase();
}
```

</details>

<details>
<summary>💡 Hint 2: Error Handling</summary>

```typescript
if (!input) {
  throw new Error('Input is required');
}
```

</details>

<details>
<summary>💡 Hint 3: Complete Solution</summary>

```typescript
export class ExampleService {
  process(input: string): string {
    if (!input) {
      throw new Error('Input is required');
    }
    return input.toUpperCase();
  }
}
```

</details>
