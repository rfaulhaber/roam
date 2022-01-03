# partial application

Partial application is a form of [[functional application]] wherein the first `n` arguments are bound.

This is not to be confused with [[currying]],

In [[JavaScript]] in particular, a built-in method on functions called `.bind` can accomplish this:

```js
const add = (x, y) => x + y;
const add10 = add.bind(null, 10);
console.log(add10(20));
```

We could implement a free function as such in [[TypeScript]]:

```typescript
type Variadic<R> = (...args: any[]) => R;

function partial<T>(fn: Variadic<T>, ...args: any[]): Variadic<T> {
    return function(...rest: any[]) {
        return fn(...args, ...rest);
    }
}

const add = (x: number, y: number): number => x + y;
const add10 = partial(add, 10);
console.log(add10(100));
```


## Backlinks

-   [[Type-Driven Development With Idris]]
