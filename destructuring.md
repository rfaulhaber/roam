# destructuring

Destructuring is a [[programming language]] feature, usually a kind of [[syntactic sugar]], where local variables are assigned to the values of an inner objects values.

Consider [[JavaScript]]:

In JavaScript, consider the following object assigned to the variable `x`:

```js
const x = {
    a: 1,
    b: 'foo'
};
```

In many programming languages, JavaScript included, it&rsquo;s conventional to assign variables to inner fields when working with those fields.

```js
const x = {
    a: 1,
    b: 'foo'
};
const a = x.a;
const b = x.b;

console.log('a', a, 'b', b);
```

Many languages therefore offer a shortcut, called destructuring. The following JavaScript is equivalent to the prior block:

```js
const x = {
    a: 1,
    b: 'foo'
};
const {a, b} = x;

console.log('a', a, 'b', b);
```
