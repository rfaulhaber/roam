# lisp timestamp

In [[Emacs Lisp]] at least, times are stored as lists like so:

```emacs-lisp
'(24814 288)
```

in the form of `(ticks . hz)`. If `hz` is `1000000000` (that&rsquo;s \\(1^{10}\\)), this represents a nanosecond resolution clock.

Alternatively, if it&rsquo;s a list of four elements, like so:

```emacs-lisp
'(24814 232 507836 148000)
```

This represents `(high low micro pico)`, which, in seconds, can be represented as:

\\(high \* 2^{16} + low + micro \* 10^{-6} + pico \* 10^{-12}\\)

or, as an [[s-expression]]:

```emacs-lisp
(defun calc-time (high low micro pico)
  (+ (* high (expt 2 16)) low (* micro (expt 10 -6)) (* pico (expt 10 -12))))

(let* ((now (current-time))
       (float-time-from-now (float-time now))
       (calc-time-from-now (apply 'calc-time now)))
  (message "float-time\t%s\ncalc-time\t%s" float-time-from-now calc-time-from-now))
```

