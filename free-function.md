# free function

A free function is a [[programming language]] construct where a [[function]] is not bound to a particular object, such as a [[method]].

In the following example, `f` is a free function while `obj.m` is a method (`obj.m` can reference `this`[^fn:1] ):

```js
function f() {
    return "f";
}

const obj = {
    m: function () {
        return "m";
    },
};
```


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> This example is somewhat misleading because in [[JavaScript]] a free function can become a method. Consider the following example:

```js
function f() {
    return this.name;
}

const obj = {
    name: 'obj',
};

console.log('free func:\t', f());
obj.f = f;

console.log('method:\t', obj.f());
```

Further, in [[JavaScript]], `this` can be set in free functions:

```js
function f() {
    return `my name is ${this.name}`;
}

console.log(f.call({name: 'foo'}));
```
