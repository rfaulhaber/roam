# Lisp in Small Pieces

tags
: [[Lisp]] [[programming languages]]

The code examples in these notes will use [[Racket]], since Racket is a mature [[Scheme]] implementation.


## 1. The Basics of Interpretation

Starts on p. 19

-   Most essential part of a Lisp interpreter surrounds the **eval** function, which takes a program as its argument and returns its output
-   The `eval` function tends to be disproportionately long compared to the rest of the interpreter
-   a variable where no binding form qualifies it (such as variables defined in `let`)
-   the opposite of a free variable, a variable defined in a qualified scope
-   The basic function signature for `eval` would be (in Scheme syntax):
    
    ```racket
      (define (eval expr env)
        ; ...
        )
    ```
-   **atoms** are called atoms because their representation is atomic
-   The first step of the `eval` function is to check if the expression is an atom, and if it&rsquo;s a symbol, look it up in the environment and return its value
-   `lookup` is a mapping between a symbol and its value, given an environment. In Haskell speak: `Symbol -> Env -> Maybe Value` (where value is some generic Lisp type)
-   In Scheme this function could be called `symbol->variable`
-   when an expression does not have a dotted pair and when that expression is a symbol, i.e. a symbol&rsquo;s representation is its own value
-   If we get an atom in `eval`, we check if it&rsquo;s a variable first, and if not we check to see if it&rsquo;s a Lisp data type
    
    ```racket
      #lang racket
      (define (atom? x)
      (and (not (null? x))
           (not (pair? x))))
    
      (define (evaluate e env)
        (if (atom? e)
            ; if it's a symbol, we look it up
            (cond ((symbol? e) (lookup e env))
                  ; otherwise we take it at face value, literally!
                  ((or (number? e) (string? e) (char? e) (boolean? e) (vector? e)) e)
                  (else (println "Cannot evaluate")))
        
            ; ...
        )
    ```
-   &ldquo;A [[dialect of Lisp]] is characterized by its set of special forms and by its library of primitive functions&rdquo;
-   a primitive function that cannot be redefined in its own language
    -   Special forms include: `lambda`, `if`, and `quote`
-   `quote` is what discriminates between a program and data in Lisp
-   A `quote` function returns its arguments as a Lisp value
-   Lisp suffers from the same problem regarding the relationship between `nil` and Booleans as [[JavaScript]] does, except `'()` is also `nil` and `false`
    -   [[Clojure]] has `true`, `false`, and `nil`
    -   [[Emacs Lisp]] has `t` and `nil`
-   `progn` evaluates a sequential list of forms
    
    ```racket
      #lang racket
      (define (eprogn exprs env)
        (if (pair? exprs)
            (if (pair? (cdr exprs))
                (begin (evaluate (car exprs) env)
                       (eprogn (cdr exprs) env))
                (evaluate (car exprs) env))
            '()))
    ```
-   a group of forms
-   A more precise way of describing [[functional application]] in terms of Lisp is that it&rsquo;s simply the case in `eval` when a list has no special operator as its first term
    
    ```racket
      #lang racket
      ; beginning ommitted
      (case (car e)) ; ...
      (else (invoke (evaluate (car e) env)
                    (elis (cdr e) env))) ; ...
    
      (define (evlis exprs env)
        (if (pair? exps)
            (cons (evaluate (car exps) env)
                  (evlis (cdr exps) env))
            '()))
    ```
    
    -   `invoke` here applies its first argument (a function) to its second argument (list of arguments for the function)

-   An implementation of `lookup`
    
    ```racket
      #lang racket
      (define (lookup id env)
        (if (pair? env)
            (if (eq? (caar env) id)
                (cdar env)
                (lookup id (cdr env)))
            (error "No such binding" id)))
    ```

-   An implementation of `update!`:
    
    ```racket
      #lang racket
      (define (update! id env value)
        (if (pair? env)
            (if (eq? (caar env) id)
                (begin (set-cdr! (car env) value)
                       value)
                (update! id (cdr env) value))
            (error "No such binding" id)))
    ```

-   `env` is a composite abstract type
    
    ```racket
      (define (extend env vars values)
        (cond ((pair? variables)
               (if (pair? values)
                   (cons (cons (car vars) (car values))
                         (extend env (cdr variables) (cdr values)))
                   (error "Too less values")))
              ((null? vars)
               (if (null? values)
                   env
                   (error "Too many values")))
              ((symbol? vars) (cons (cons vars values) env))))
    ```

-   The book suggests that the easiest way to define functions is to use the functions of the implementation language
    
    ```racket
      (define (invoke fn args)
        (if (procedure? fn)
            (fn args)
            (wrong "Not a function" fn)))
    ```
    
    -   This may be easy with Scheme but not so easy with a language like Rust

-   `make-function` definition
    
    ```racket
      #lang racket
      (define (make-function vars body env)
        (lambda (values)
          (eprogn body (extend env vars values))))
    ```

-   Not all Lisps use lexical binding
    -   **Lexical:** Function evaluates its body in its own definition environment
    -   **Dynamic:** Function extends the current environment
        -   JavaScript&rsquo;s `var` is an example of dynamic binding
-   The use of dynamic binding is in forward-looking computations
-   region of code where a variable is accessible
-   &ldquo;Pure&rdquo; Scheme only uses one kind of binding: `lambda`
    -   Again, JavaScript `var`
-   when one variable hides another because they both have the same name
    -   A bug in early Lisp, this returned `(2 3)`
        
        ```racket
            #lang racket
            (let ((a 1))
               ((let ((a 2)) (lambda (b)
                               (list a b))))
               3)
        ```

-   property a language has when substituting an expression in a program for an equivalent expression that does not change the behavior of the program
    
    ```racket
      #lang racket
      (eq?
       (let ((x (lambda () 1))) (x))
       ((let ((x (lambda () 1))) x)))
    ```

-   When the current environment is represented as an association list (or hash map), where lookup time is not constant. It favors the environment and programming by multi-tasking to the detriment of searching for values and variables
-   Each variable is associated with a place where its value is always stored independently of the current environment, such as in a lookup table. It favors searching for values of variables to the detriment of function calls
-   These are implementation details and not semantic ones
-   A global environment provides a **library** of primitive, useful functions
    -   These include `car`, `cons`, arithmetic
-   a binding that cannot change. `t` and `nil` are such examples
-   the opposite of immutable binding!


## 2. Lisp, 1, 2, &#x2026; ω

-   In Lisp (Scheme) `lambda` is the means for building new functions
-   Function application is the bare minimally legal operation of functions
-   anything that can be stored in a variable
-   The Lisp2 interpreter implemented in this book uses a separate `evaluate` function for functions, called `f.evaluate`
-   A Lisp should be able to support function application, making code like this possible:
    
    ```racket
      #lang racket/base
      ((if #t + *) 3 4)
    ```
    
    -   note: this code does not work in Emacs Lisp
        
        ```elisp
            ((if t + *) 3 4)
        ```
-   The conundrum that the author is presenting in this chapter thus far is:
    
    > To summarize these problems, we should say that there are calculations belonging to the parametric world that we want to carry out in the function world, and vice versa. More precisely, we may want to pass a function as an argument or as a result, or we may even want the function that will be applied to be the result of a lengthy calculation. (p. 36)
-   We need a function `funcall` that applies its first argument to its other arguments. In Racket:
    
    ```racket
      (define (funcall . args)
        (if (> (length args) 1)
            (invoke (car args) (cdr args))
            (error "Incorrect arity" 'funcall)))
    ```
-   The current problem with this implementation is that it would treat `+` or `*` as variables and not functions
    -   That is, we want: `(if condition (+ 3 4) (* 3 4))` ≡ `(funcall (if condition (function +) (function *)) 3 4)`
-   We now need a `function` case in `evaluate`, which will convert the name of a function into a functional value
    
    ```racket
      #lang racket/base
      ;...
      ((function)
       (cond ((symbol? (cadr e))
              (lookup (cadr e) fenv))
             (else (error "Incorrect function" (cadr e)))))
    ```
-   `flet`, _functional let_, allows us to extend the function environment, in the same way that `let` allows us to extend the environment of variables
    -   its syntax is similar to `let`, except each binding takes an extra argument that is a function body
-   `flet` would extend the `f.evaluate` as such:
    
    ```racket
      #racket
      ; ...
      ((flet)
       (f.eprogn
        (cddr e)
        env
        (extend fenv
                (map car (cadr e))
                (map (lambda (def)
                       (f.make-function (cadr def) (cddr def) env fenv))
                     (cadr e)))))
    ```
-   `flet` lets us write functions like:
    
    ```racket
      (flet ((square (x) (* x x)))
            (lambda (x) (square (square x))))
    ```
-   Once the evaluator can handle function terms, we could write functions in such a way that, for example, integers could be used as accessors to lists
    
    ```racket
      (2 '(foo bar baz quux)) ; -> baz
      (-2 '(foo bar baz quux)) ; -> (baz quux)
    ```
-   Alternatively we could apply a list of functions
    
    ```racket
      ((list + - *) 5 3) ; -> (8 2 15)
      ; equivalent to:
      (map (lambda (f) (f 5 3)) (list + - *))
    ```
-   Although these are interesting, the author leaves us with a disclaimer:
    
    > Finally, we could even allow the function to be in the second position in order to simulate infix notation. In that case, A + 2) should return 3. dwim (that is, Do What I Mean) in [Tei74, Tei76] knows how to recover from that kind of situation. All these innovations are dangerous because they reduce the number of erro- erroneous forms and thus hide the occurrence of errors that would otherwise be easily detected. Furthermore, they do not lead to any appreciable savings in code, and when everything is taken into account, these innovations are actually rarely used. They also remove that affinity between functions and applicable functional objects, that is, the objects that could appear in the function position. With these inno- innovations, a list or a number would be applicable without so much as becoming a function itself. As a consequence, we could add applicable objects without raising an error, like this:
    
    ```racket
      (apply (list 2 (list 0 (+ 1 2)))
             '(foo bar baz quux))
      ; -> (baz (foo quux))
    ```
-   It&rsquo;s a good idea to prevent naming functions or variables after special forms, e.g. `lambda`
-   bindings between names and the entities referenced by those names
-   A namespace is a particular kind of environment
-   A Lisp implementation should implement as few special forms as possible
-   A Lisp that implements multiple namespaces for values needs to have mechanisms to distinguish them. The author goes on a long divergence about dynamic binding, and how it explicitly differs from lexical binding
-   A problem we haven&rsquo;t run into yet is that of recursion. In our toy interpreters, consider the following:
    
    ```racket
      #lang racket
      (set! fact (lambda (n)
                   (if (= n 1) 1
                       (* n (fact - n 1)))))
    ```
    
    How can a value reference itself like this?
-   In order to make that definition of `fact` legal we need to handle values that don&rsquo;t exist yet
-   Lisp assumes that at most only one global variable can exist with a given name, and that variable is visible everywhere
-   &ldquo;Simple recursion demands a global environment&rdquo;
-   A function references itself
-   Two or more functions recursively reference each other
    -   For example, `odd?` and `even`
        
        ```racket
            #lang racket
            (define (even? n)
              (if (= n 0) #t (odd? (- n 1))))
        
            (define (odd? n)
              (if (= n 0) #f (even? (- n 1))))
        ```
-   Defining recursive functions locally also introduces problems
-   Aside: `let` can be defined in terms of `lambda` using macros
-   The solution to uninitialized variables is to give them a kind of `undefined` value


### Source code reference


#### `f.evaluate`

```racket
(define (f.evaluate e env fenv)
  (if (atom? e)
      (cond ((symbol? e) (lookup e env))
            ((or (number? e) (string? e) (char? e)
                 (boolean? e) (vector? e) )
             e )
            (else (wrong "Cannot evaluate" e)) )
      (case (car e)
        ((quote)  (cadr e))
        ((if)     (if (f.evaluate (cadr e) env fenv)
                      (f.evaluate (caddr e) env fenv)
                      (f.evaluate (cadddr e) env fenv) ))
        ((begin)  (f.eprogn (cdr e) env fenv))
        ((set!)   (update! (cadr e)
                           env
                           (f.evaluate (caddr e) env fenv) ))
        ((lambda) (f.make-function (cadr e) (cddr e) env fenv))
        (else     (evaluate-application (car e)
                                        (f.evlis (cdr e) env fenv)
                                        env
                                        fenv )) ) ) )

(define (f.evlis exps env fenv)
  (if (pair? exps)
      (cons (f.evaluate (car exps) env fenv)
            (f.evlis (cdr exps) env fenv) )
      '() ) )

(define (f.eprogn exps env fenv)
  (if (pair? exps)
      (if (pair? (cdr exps))
          (begin (f.evaluate (car exps) env fenv)
                 (f.eprogn (cdr exps) env fenv) )
          (f.evaluate (car exps) env fenv) )
      empty-begin ) )

(define (f.make-function variables body env fenv)
  (lambda (values)
    (f.eprogn body (extend env variables values) fenv) ) )

(define (lookup id env)
  (if (pair? env)
      (if (eq? (caar env) id)
          (cdar env)
          (lookup id (cdr env)) )
      (wrong "No such binding" id) ) )

(define (update! id env value)
  (if (pair? env)
      (if (eq? (caar env) id)
          (begin (set-cdr! (car env) value)
                 value )
          (update! id (cdr env) value) )
      (wrong "No such binding" id) ) )

(define (extend env variables values)
  (cond ((pair? variables)
         (if (pair? values)
             (cons (cons (car variables) (car values))
                   (extend env (cdr variables) (cdr values)) )
             (wrong "Too less values") ) )
        ((null? variables)
             (if (null? values)
                 env
                 (wrong "Too much values") ) )
        ((symbol? variables) (cons (cons variables values) env)) ) )

(define (invoke fn args)
  (if (procedure? fn)
      (fn args)
      (wrong "Not a function" fn) ) )

(define (evaluate-application fn args env fenv)
  (cond ((symbol? fn)
         (invoke (lookup fn fenv) args) )
        ((and (pair? fn) (eq? (car fn) 'lambda))
         (f.eprogn (cddr fn)
                   (extend env (cadr fn) args)
                   fenv ) )
        (else (wrong "Incorrect functional term" fn)) ) )
```


## 3. Escape & Return: Continuations

-   Continuations come in two forms:
    -   **Continuation:** abstract representation of control state
        -   had to look this up, much more uncommon now it seems
        -   see:
            -   [Continuation - Wikipedia](https://en.wikipedia.org/wiki/Continuation)
            -   [functional programming - Continuations in Clojure - Stack Overflow](https://stackoverflow.com/questions/1173133/continuations-in-clojure)
            -   [Beautiful Racket: Continuations](https://beautifulracket.com/explainer/continuations.html)
    -   **Current continuation:** a continuation that can be derived from the current state of the program
-   continuations were implemented as a kind of Lisp equivalent for `goto`
    
    ```common-lisp
      (defun fact (n)
        (prog (r)
              (setq r 1)
              loop (cond (( = n 1 ) (return r)))
              (setq r (* n r))
              (setq n (= n 1))
              (go loop)))
    ```
    
    -   this example reminds me of [[Clojure]]&rsquo;s `recur` form
-   continuations can be used to define throw/catch
-   Aside: a Lisp interpreter is essentially a function mapping forms and environment to an output
-   For the interpreter used in this chapter, we use a function called `define-generic`, which would be used in the following manner:
    
    ```racket
      (define-generic (invoke (f) v r k)
        (wrong "not a function" f r k))
    ```
    
    -   I wonder if it&rsquo;s worth defining such a function in Rust code

-   `evaluate` for this interpreter is also relatively simple:
    
    ```racket
      ; e = expression
      ; r = env
      ; k = continuation
      (define (evaluate e r k)
        (if (atom? e)
            (cond ((symbol e) (evaluate-variable e r k))
                  (else (evaluate-quote e r k)))
            (case (car e)
              ((quote) (evaluate-quote (cadr e) r k))
              ((if) (evalaute-if (cadr e) (caddr e) (cadddr e) r k))
              ((begin) (evaluate-begin (cdr e) r k))
              ((set!) (evaluate-set! (cadr e) (cddr e) r k))
              ((lambda) (evaluate-lambda (cadr e) (cddr e) r k))
              (else (evaluate-application (car e) (cdr e) r k)))))
    ```
-   here, we only need three functions, `evaluate`, `invoke`, and `resume`
-   I will not copy all the functions defined in this chapter, but it should be referenced when building out an interpreter. May be useful!
-   a call at the end of a function
-   when a function recursively calls itself as the last step in the function
-   Continuations are a bit like execution stacks in other languages
-   Implementing tail-call recursion with continuations would be a matter of reusing the current continuation


## 4. Assignment and Side Effects

-   The problem of assignment is hard, because Lisp code can have ambiguities with regards to what should be allowed
    
    ```racket
      (let ((name "Nemo"))
        (set! winner (lambda () name))
        (set! set-winner! (lambda (new-name) (set! name new-name)
                            name))
        (set-winner! "Me")
        (winner))
    ```
    
    What should be returned from this? Should `set-winner!` be allowed to update `name`?

-   A [box](https://docs.racket-lang.org/reference/boxes.html?q=box#%28def._%28%28quote._~23~25kernel%29._box%29%29) is a data structure in Lisp that allows for mutable references
-   Implementing a box is apparently trivial
    
    ```racket
      #lang racket
      (define (make-box value)
        (lambda (msg)
          (case msg
            ((get) value)
            ; this part is not so easy in a language like Rust
            ((set!) (lambda (new-value) (set! value new-value))))))
    
      (define (box-ref box)
        (box 'get))
      (define (box-set! box new-value)
        ((box 'set!) new-value))
    
      (define my-box (make-box 123))
      (println (box-ref my-box))
      (box-set! my-box 321)
      (println (box-ref my-box))
    ```
-   Boxes are preferable when shared variables have to be mutable
-   The global environment could be thought of as a giant `let` form that wraps the entire program
-   one where references to all named values must exist at the time of reference
-   note: in Racket, `letrec` is used when a `let` binding requires two mutually used definitions, like so:
    
    ```racket
      (letrec ([is-even? (lambda (n)
                           (or (zero? n)
                               (is-odd? (sub1 n))))]
               [is-odd? (lambda (n)
                          (and (not (zero? n))
                               (is-even? (sub1 n))))])
        (is-odd? 11))
    ```
    
    `define` makes this superfluous, however


## 5. Denotational Semantics


### [[λ-calculus]] crash course


#### Variable

Akin to a function argument \\(\forall x \in Variable, x \in \Lambda\\)


#### Abstraction

Akin to a function body \\(\forall x \in Variable, \forall M \in \Lambda, \lambda x . M \in \Lambda\\)


#### Combination

\\(\forall M,N \in \Lambda, (M N) \in \Lambda\\)


#### β-reduction

When we apply a function with a body \\(M\\) and a variable \\(x\\) to a term \\(N\\), we get a new term which is the body of \\(M\\) of the function with the variable \\(x\\) replaced by the term \\(N\\), so:

\\(M[x \longrightarrow N]\\)

Further

\\((\lambda x.M N) \xrightarrow{\beta} M[x \rightarrow N]\\)

A **redex** is a reducible expression.


### Chapter notes

-   Each part of a Lisp program could be thought of as a function (aside: like in [[category theory]]?)
    -   **Environment:** Identifier -> address
    -   **Memory:** Address -> value
    -   **Value:** Function | Boolean | Integer | Pair | &#x2026;
    -   **Continuation :** Value x Memory -> Value
    -   **Function:** Value x continuation x memory -> value
-   \\(\forall x, f(x) = g(x) \Rightarrow (f = g)\\)
-   I&rsquo;m skipping this chapter because I don&rsquo;t know lambda calculus


## 6. Fast Interpretation

In order to speed up interpretation, an _[[activation record]]_ will be used to provide a function with its arguments.

The function signature for the interpreter is essentially:

\begin{document}
meaning: \text{Program} x \text{Environment} \rightarrow \text{(Activation-Record x Continuation)} \rightarrow \text{Value}
\end{document}

Where `Program x Environment` is static and `Acivation-Record x Continuation` is dynamic.

For convenience, I&rsquo;ve reproduced the notation conventions used in the book here:

| Notation           | Meaning                              |
|--------------------|--------------------------------------|
| e                  | Expression, form                     |
| r                  | Environment                          |
| sr, &#x2026; , v\* | Activation record                    |
| v                  | Value (integer, pair, closure, etc.) |
| k                  | Continuation                         |
| f                  | Function                             |
| n                  | Identifier                           |

The fast interpreter begins with defining various `meaning` functions, which resolve input to a value.

`meaning-quotation` is defined as such:

```racket
(define (meaning-quotation v r)
  (lambda (sr k)
    (k v)))
```

The fast interpreter stores global variables and local variables separately.

The fast interpreter is generally faster because it pre-treats the values in a compiler-like manner, avoiding unnecessary memory allocations.

Closures are handled as such:

```racket
(struct closure (code closed-environment))

(define (invoke f v*)
  (if (closure? f)
      ((closure-code f) v* (closure-closed-environment f))
      (wrong "Not a function" f)))
```

Although this chapter complicates the interpreters written thus far considerably, it does also speed up interpretation.
