# prototype chain

A prototype chain is a way to inherit methods in [[JavaScript]]. You can think of it as working like a [[linked list]].

Consider the following:

```js
const Obj = function (name) {
  this.name = name;
};

Obj.prototype.hello = function () {
  return `Hello, my name is ${this.name}`;
};

const o = new Obj("Fido");

const newProto = Object.create(Obj.prototype);

const NewObj = function (name) {
  this.name = name;
};

NewObj.prototype = newProto;

const oo = new NewObj("Mike");

NewObj.prototype.bark = function () {
  return `${this.hello()}, and I bark!`;
};

console.log(oo.hello());
console.log(oo.bark());

console.log(typeof o.bark);
```

The `extends` keyword in JavaScript does the same as the above.

```js
class Obj {
  constructor(name) {
    this.name = name;
  }

  hello() {
    return `Hello, my name is ${this.name}`;
  }
}

class NewObj extends Obj {
  constructor(name) {
    super(name);
  }

  bark() {
    return `${this.hello()}, and I bark!`;
  }
}

const oo = new NewObj("Mike");

console.log(oo.hello());
console.log(oo.bark());
```

