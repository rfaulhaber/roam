# dialect of Lisp

tags
: [[Lisp]]

Except for perhaps the original Lisp in the 1950s, there is no &ldquo;pure Lisp,&rdquo; only variations. Each dialect is distinguished by its particular syntax and its library functions. For example, Scheme has a `define` function that takes a list as its first argument, and in this way you can define a function.

```scheme
(define (foo arg1 arg2)

    )
```

However other Lisps, such as [[Emacs Lisp]] or [[Clojure]] define functions as such:

```emacs-lisp
(defun foo (arg1 arg2)
  ; body...
  )
```

