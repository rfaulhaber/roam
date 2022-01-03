# Dial interpreter third implementation

tags
: [[Dial implementation]] [[language design]]

I have now attempted to implement Dial twice, both as spins on the Make A Lisp project. For my third and hopefully final attempt, I&rsquo;m going to try a different approach, using lessons taken from [[Lisp in Small Pieces]].

Dial will once again be written in [[Rust]].


## Plan


### Syntax

Dial&rsquo;s syntax will mostly be [[Clojure]]-like S-expressions, with some exceptions. It will use Clojure&rsquo;s `def` and `fn` keywords. List and vector syntax will be interchangeable, as in [[Racket]].

```lisp
(let (a 1
      b 2
      c 3)
  (+ a b c))
```

Bindings may optionally be bracketed.

```lisp
(let ((a 1)
      [b 2]
      c 3)
  (+ a b c))
```


### Scoping rules

All named values are immutable. `let` bindings, function parameters, etc. cannot be mutated.


### Types

Dial&rsquo;s types will mostly be Clojure-like as well. We will not define cons cells, since linked lists in Rust are superfluous.


#### Primitives

-   `true` and `false`
-   Integers
-   Floating point numbers
-   Strings
-   Lists (e.g. `'(1 2 3)`)
    -   All lists will also be iterable and take advantage of Rust&rsquo;s iteration features
-   Keywords (e.g. `:foo`)
-   Symbols (quoted values, e.g. `'Apple`)


##### Deferable

These types may come after v1, since they could be articulated with lists.

-   Vectors (e.g. `[1 2 3]`)
-   Hash maps (e.g. `{a 1 b 2}`)


### Parser (`read`)

I have largely already figured out how the parser should work thanks to `nom`. The only forms I don&rsquo;t support at the moment are quoted expressions and a handful of special syntax types like hash maps.

A valid program that&rsquo;s outputted by `read` will return a quoted expression.


### `eval`

The interpreter should be thought of as a function that, given a program, an environment, and a [[continuation]], should result in a value.


## Should I implement continuations?


### Pros

-   Interesting language feature
-   Will allow for implementation of tail call optimization


### Cons

-   Difficult to implement in Rust
-   Possibly memory intensive if all done on the heap
-   Not especially useful / can live without it
