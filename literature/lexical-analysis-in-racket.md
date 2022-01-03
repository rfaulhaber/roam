# Lexical Analysis in Racket

source
: [Lexical analysis in Racket](http://matt.might.net/articles/lexers-in-racket/)

tags
: [[lexical analysis]]


## Notes

[[Racket]] has a library for making [[lexers]] easily: `parser-tools/lexer`.

```racket
#lang racket/base
(require parser-tools/lexer)

(define ab-lexer 
 (lexer 
   [#\a  (display "You matched a.\n")]
   [#\b  (display "You matched b.\n")]))
```

This will create a lexer that matches input character by character until, finally, returning `'eof`.

The rest of this article contains a number of useful examples.
