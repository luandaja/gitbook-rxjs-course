---
description: Friendly reminder
---

# Installation

For the course we will use RxJs 6.x

To install follow the repository steps \(from rxjs repository\):

## Installation and Usage

### ES6 via npm

```bash
npm install rxjs
```

It's recommended to pull in the Observable creation methods you need directly from `'rxjs'` as shown below with `range`. And you can pull in any operator you need from one spot, under `'rxjs/operators'`.

```javascript
import { range } from 'rxjs';
import { map, filter } from 'rxjs/operators';

range(1, 200).pipe(
  filter(x => x % 2 === 1),
  map(x => x + x)
).subscribe(x => console.log(x));
```

Here, we're using the built-in `pipe` method on Observables to combine operators. See [pipeable operators](https://github.com/ReactiveX/rxjs/blob/master/doc/pipeable-operators.md) for more information.

### CommonJS via npm

To install this library for CommonJS \(CJS\) usage, use the following command:

```bash
npm install rxjs
```

\(Note: destructuring available in Node 8+\)

```javascript
const { range } = require('rxjs');
const { map, filter } = require('rxjs/operators');

range(1, 200).pipe(
  filter(x => x % 2 === 1),
  map(x => x + x)
).subscribe(x => console.log(x));
```

### CDN

For CDN, you can use [unpkg](https://unpkg.com/):

[https://unpkg.com/rxjs/bundles/rxjs.umd.min.js](https://unpkg.com/rxjs/bundles/rxjs.umd.min.js)

The global namespace for rxjs is `rxjs`:

```javascript
const { range } = rxjs;
const { map, filter } = rxjs.operators;

range(1, 200).pipe(
  filter(x => x % 2 === 1),
  map(x => x + x)
).subscribe(x => console.log(x));
```

## Additional resources

1. [Rxjs github](https://github.com/ReactiveX/rxjs/tree/6.x)

