## Bonus Challenges

### Challenge 1: Multiple Transformations

Extend the service to support different transformation modes:

```typescript
type TransformMode = 'uppercase' | 'lowercase' | 'capitalize';

process(input: string, mode: TransformMode = 'uppercase'): string {
  // Implement all three modes
}
```

### Challenge 2: Async Processing

Make the method async and add a simulated delay:

```typescript
async processAsync(input: string): Promise<string> {
  await new Promise(resolve => setTimeout(resolve, 100));
  return this.process(input);
}
```

### Challenge 3: Unit Tests

Write proper unit tests using your preferred testing framework:

```typescript
describe('ExampleService', () => {
  it('should transform to uppercase', () => {
    const service = new ExampleService();
    expect(service.process('hello')).toBe('HELLO');
  });

  it('should throw on empty input', () => {
    const service = new ExampleService();
    expect(() => service.process('')).toThrow();
  });
});
```
