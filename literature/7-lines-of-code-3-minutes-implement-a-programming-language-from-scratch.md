# 7 lines of code, 3 minutes: Implement a programming language from scratch

source
: [7 lines of code, 3 minutes: Implement a programming language](http://matt.might.net/articles/implementing-a-programming-language/)


## Notes

[[Lambda calculus]] has three fundamental operations: variable references, anonymous functions and function calls.

\\(\lambda v . e\\) is comperable to `(v) => e` in JavaScript. \\((f e)\\) is a function call.

If we wanted to define a lambda calculus interpreter, it would be trivial to express that:

```text
 eval  : Expression * Environment -> Value
 apply : Value * Value -> Value

 Environment = Variable -> Value
 Value       = Closure
 Closure     = Lambda * Environment
```

It also has a trivial implementation in [[Racket]]:

```racket
#lang racket

; bring in the match library:
(require racket/match)

; eval matches on the type of expression:
(define (eval exp env)
  (match exp
    [`(,f ,e)        (apply (eval f env) (eval e env))]
    [`(λ ,v . ,e)   `(closure ,exp ,env)]
    [(? symbol?)     (cadr (assq exp env))]))

; apply destructures the function with a match too:
(define (apply f x)
  (match f
    [`(closure (λ ,v . ,body) ,env)
     (eval body (cons `(,v ,x) env))]))

; read in, parse and evaluate:
(display (eval (read) '()))    (newline)
```

The rest of this post has a longer interpreter and some worthwhile links.

