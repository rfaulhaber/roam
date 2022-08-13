# for loops considered harmful

tags
: [[programming languages]]

I think I learned this either from [[Rust]] or [[functional programming]], but there&rsquo;s an idea that `for` loops are considered harmful.


## The danger

Consider the following:

```go
days := make([]int, 30)

for i = 0; i < 31; i++ {
	days[i] = i
}
```

This is a banal example, but this code introduces the possibility for an indexing error.


## The solution

There are a few ways to mitigate these problems.


### Mapping

Most for loops you see in the wild are just mappings of some kind or another. Mapping is when you want a 1:1 derivation of a new value from an old value.

```js
const numbers = [1, 2, 3, 4, 5];
const areOddOrEven = [];

for (let i = 0; i < numbers.length; i++) {
    areOddOrEven[i] = numbers[i] % 2 === 0;
}

console.log(areOddOrEven);
```

This example is a mapping from number to bool, where what we&rsquo;re mapping is whether or not a number is even. It could be rewritten as:

```js
const numbers = [1, 2, 3, 4, 5];
const areEvenOrOdd = numbers.map(v => v % 2 === 0);
console.log(areEvenOrOdd);
```

Mapping, in general, could be implemented as such:

```js
function map(arr, fn) {
    const res = [];
    for (let i = 0; i < arr.length; i++) {
        res[i] = fn(arr[i]);
    }

    return res;
}
```


### Reduction

`reduce` functions are thought of as &ldquo;summary&rdquo; or &ldquo;rollup&rdquo; functions.

One example is adding values.

```js
const numbers = [1, 2, 3, 4, 5];
let sum = 0;
for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
}
console.log(sum);
```

This could be just as easily written as:

```js
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((sum, val) => sum + val, 0);
console.log(sum);
```

This could be generally implemented as:

```js
function reduce(coll, fn, init) {
    let res = init;

    for (let i = 0; i < coll.length; i++) {
        res = fn(res, coll[i]);
    }

    return res;
}

const numbers = [1, 2, 3, 4, 5];
const sum = reduce(numbers, (sum, val) => sum + val, 0);
console.log(sum);
```


### Filtering


### Iterators

