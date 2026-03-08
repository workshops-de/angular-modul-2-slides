# Simple Usage

<WindowMockup codeblock title="[file name]">

```ts
@Component({})
export class MyGrid {}
```

</WindowMockup>

# Steps
- Shiki supports highlighting
  - all lines `*`
  - single line e.g `1`
  - line range e.g `1-2``
  - multiple lines e.g. `1,7``
- Example `{*|1|1,6|2-4|*}``

<WindowMockup codeblock title="[file name]">

```ts{*|1|2|1-2|*}
@Component({})
export class MyGrid {}
```

</WindowMockup>

# Annotations
- with twoslash plugin annotations are possible for @log, @error, @warn, @annotate
- gives context to code example

<WindowMockup codeblock title="code.ts">

```ts {*} twoslash
// @log: Custom log message
const a = 1;
// @error: Custom error message
const b = 1;
// @warn: Custom warning message
const c = 1;
// @annotate: Custom annotation message
```

</WindowMockup>
