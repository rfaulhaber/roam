# currying

Currying, in programming, is a [[functional programming]] technique whereby a [[function]] of `n` arity is reduced to `n` unary functions. The term comes from [[lambda calculus]].

```js
const add = (x, y, z) => x + y + z;
const curriedAdd = curry(add);
curriedAdd(1)(2)(3) === curriedAdd(1, 2)(3) === curriedAdd(1)(2, 3) === curriedAdd(1, 2, 3);
```

Currying could be implemented in [[JavaScript]] as such:

```js
function curry(func) {
  const arity = func.length;

  return function wrapper(...args) {
    if (args.length >= arity) {
      return func(...args);
    } else {
      return function(...otherArgs) {
        return wrapper(...args.concat(otherArgs));
      };
    }
  };
}

const add = (x, y, z) => x + y + z;
const curriedAdd = curry(add);
console.log(curriedAdd(1)(2)(3) === 6);
console.log(curriedAdd(1, 2)(3) === 6);
console.log(curriedAdd(1)(2, 3) === 6);
console.log(curriedAdd(1, 2, 3) === 6);
```

In [[Haskell]], all functions are automatically curried:

```haskell
add x y z = x + y + z

add10 = add 10

add10 100 2
```
