# Lisp primitives

tags
: [[Lisp]] [[programming languages]] [[Dial]] [[Dial implementation]]

When implementing a Lisp in particular, the goal should be to express the totality of the Lisp with only a minimal set of implementation language code. For example, it wouldn&rsquo;t make a lot of sense to implement `when` in the source language (say, C) when, if `if` is implemented, it could be expressed as `if`[^fn:1].

Primitives include but are not limited to:

-   Basic data types
-   Any arithmetic
-   `lambda` or equivalent
-   `if`
-   `cond`
-   `list`
-   `apply`
-   `defmacro`
-   `quote`

The rest of a Lisp is simply library code, which can be expressed using all of the above.


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> In fact, it could be expressed like this:

```emacs-lisp
; a copy of emacs's own =when= definition
(defmacro my-when (condition &rest body)
  (list 'if condition (cons 'progn body)))

(macroexpand '(my-when t
         (message "hello world I did it")))
```
