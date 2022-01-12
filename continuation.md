# continuation

Continuations are a [[Lisp]] convention that, more or less, represent the current stack frame of a program.

Continuations are used in [[Scheme]] in particular, as part of their [[compiler]] implementation.

In Scheme, continuations are first class, and can be used using the function `call/cc`.

Continuations make the use of a call-stack redundant.
