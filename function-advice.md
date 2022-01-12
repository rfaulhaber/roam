# function advice

[[Emacs Lisp]] has a language feature called function advice, allowing users to modify other functions that have been defined already.

Consider the following:

```emacs-lisp
(defun foo (x)
  (+ 10 x))

(message "before advice: %s" (foo 3))

(advice-add 'foo :filter-return (lambda (x)
                                  (* x 2)))

(message "after advice: %s" (foo 3))
```

For official docs on combinators, see [this](https://www.gnu.org/software/emacs/manual/html_node/elisp/Advice-Combinators.html).
