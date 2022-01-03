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


## Backlinks

-   [[object-oriented programming]]
