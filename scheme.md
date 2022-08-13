# Scheme

Scheme is a popular [[dialect of Lisp]], and has many descendants including [[Racket]].


## Misc

In Scheme, a \* in a function name means &ldquo;repeat throughout a list.&rdquo;

Although [[Emacs Lisp]] is not a Lisp, it has two forms of `let`: `let` and `let*`, where the latter lets you write code like this:

```emacs-lisp
(let* ((foo "foo")
       (bar (concat foo "bar")))
  (message "%s" bar))
```

In this example, `foo` is able to be used in the definition of `bar`, while if this were a `let` form, this would report an error.

