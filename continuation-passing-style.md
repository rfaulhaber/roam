# continuation-passing style

[[Continuation]]-passing style is a style of (Lisp) programming where control flow continues through the passing of functions.

Typically, it looks like this:

```racket
#lang racket

(define (add x y k)
  (k (+ x y)))
```

This would pass the control flow to `k`. The idea, then, is that your whole program threads through in this manner. The interpreter would define the first lambda and pass it in as `k`.

```racket
#lang racket

(define (add x y k)
  (k (+ x y)))

(add 1 2 (lambda (n) (+ 3 n)))
```

The interpreter could thus use continuations for all calls.

[[Scheme]] in particular is implemented in such a way that you can use the current continuation directly by calling `call/cc`, or &ldquo;call with current continuation.&rdquo;
