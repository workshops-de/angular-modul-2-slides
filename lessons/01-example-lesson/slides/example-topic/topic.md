---
layout: cover
logo: lessons/01-example-lesson/slides/example-topic/Icon.png
---

# Your Workshop

Succeed, whatever you build

---
layout: section
---

# First Section

---
layout: sub-section
---

# First Sub-Section

---

# Hallo

**Strong**, _italic_, normal

---
layout: why
---

# Explains, why this topic matters.

---
layout: little-what
---

# Explain in one sentence what the following topic is about.

---
layout: what-if
---

# What if we want to bring it to the next level?

---
layout: task
---

# Create your own solution, now.

---

# Explanation

- Work with lists
- to explain important things
- step by step
- lists are easy to follow

---

<tldraw class="inset-0 w-full h-full" doc="01-example-lesson/example-topic/full-screen-drawing-example.json"></tldraw>

---
layout: section
---

# Code Examples

---

# Simple Code Example

<WindowMockup codeblock title="my-file.js" padding="2rem">
```js
export function do() {
    return 'Success 🚀';
}
```
</WindowMockup>

### Markup

````html
<WindowMockup codeblock title="my-file.js" padding="2rem">
  ```js export function do() { return 'Success 🚀'; } ```
</WindowMockup>
````

---

# Magic move

<WindowMockup codeblock>

````md magic-move
```ts
console.log('Hello, World!');
```

```js
console.log('Hello, World!');
console.log(`Step ${1}`);
```

```js
console.log('Hello, World!');
console.log(`Step ${1 + 1}`);
```

```ts
console.log('Hello, World!');
console.log(`Step ${3}` as string);
```
````

</WindowMockup>

---
layout: two-cols-header
layoutClass: gap-x-2
---

# Side-by-side

::left::

<WindowMockup codeblock title="template.html">

```html
<p>Template</p>
```

</WindowMockup>

::right::

<WindowMockup codeblock title="code.ts">

```ts {all} twoslash
// @log: Custom log message
const a = 1;
// @error: Custom error message
const b = 1;
// @warn: Custom warning message
const c = 1;
// @annotate: Custom annotation message
```

</WindowMockup>

::bottom::
<Callout type="info">
Shikijs and twoslash rock your code samples
</Callout>

---
layout: ask-me-anything
---
