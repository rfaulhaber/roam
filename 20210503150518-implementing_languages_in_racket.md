# implementing languages in Racket

tags
: [[programming language implementation]] [[Racket]]

[[Racket]] has a lot of tools for implementing new programming languages.


<a id="org5b84ee4"></a>

## Process

In general, there are two steps to implement a new language in Racket: you must first implement a reader and then an expander.


<a id="orga15c51d"></a>

### Reader

The reader is like a parser. It takes text and converts it into [[s-expressions]].


<a id="orgdbe03ca"></a>

### Expander

The expander is what either generates Racket code or otherwise how the s-expressions from the reader correspond to Racket code.


<a id="orgae36258"></a>

## Links

-   [Creating Languages · racket/racket Wiki · GitHub](https://github.com/racket/racket/wiki/Creating-Languages)
-   [17.3 Defining new #lang Languages](https://docs.racket-lang.org/guide/hash-languages.html)
