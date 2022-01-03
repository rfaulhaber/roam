# macros

Macros are a feature in certain [[programming languages]] that allow for editing source code at compile or runtime. Similar to functions, macros are a means of code reuse, but rather than rewrite functionality they rewrite code.

Macros first appeared, to my knowledge, in [[Lisp]].

In Lisp (specifically [[Emacs Lisp]]), a macro looks like this:

```emacs-lisp
(defmacro ++ (var)
  "Incrementing operator like in C."
  (list 'setq var (list '+ 1 var)))

(let ((my-var 1))
  (++ my-var))
```

The above, at runtime, is expanded in the following manner:

```emacs-lisp
(macroexpand '(++ foo))
```

(setq foo (+ 1 foo))

Consider also the following example in [[Rust]]:

```rust
macro_rules! inc {
    ($name:ident) => {
        $name = $name + 1
    }
}

fn main () {
    let mut foo = 1;
    inc!(foo); // macros in rust end with exclamation points
    println!("foo: {}", foo); // println is also a macro
    // => 2
}

```

Macros allow for [[programming language extension]].


## Backlinks

-   [[programming language extension]]
