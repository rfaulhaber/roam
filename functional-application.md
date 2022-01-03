# functional application

Functional application is the act of applying arguments to a [[function]]. In programming this is usually distinct from simply calling a function.

In a [[Lisp]], usually this means taking an actual list of arguments and calling a function with it:

```emacs-lisp
(apply 'car '((1 2 3)))
```

Compare that with:

```emacs-lisp
(car '(1 2 3))
```

In the first instance, the outer list is supplying the list of arguments to `car`.

In [[Lisp]], functional application is the cornerstone of a Lisp interpreter, where often in the source code a function&rsquo;s arguments are quite literally applied to it.

Or, consider the following in [[JavaScript]]:

```js
const add = (x, y, z) => x + y + z;
console.log(add.apply(null, [1, 2, 3]));
```


## Backlinks

-   [[Lisp in Small Pieces]]
-   [[partial application]]
