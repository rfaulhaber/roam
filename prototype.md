# prototype

Prototypes are a language construct in [[JavaScript]] that allow for [[inheritence]].

JavaScript only has objects, which are string-based key-value pairs. A method in JavaScript is basically just a key-value pair where the value is a function type (with a special context).

Every object has a prototype.

```js
// this is a constructor for an object
let Obj = function (name) {
  this.name = name;
};

const o = new Obj("Fido");

console.log("does o have a hello function?", typeof o.hello);

// we can modify the prototype at runtime
Obj.prototype.hello = function () {
  return `Hello, my name is ${this.name}`;
};

console.log("does o have a hello function now?", typeof o.hello);

console.log("what happens when we call o?", o.hello());
```

Prototype inheritence is called a [[prototype chain]].
