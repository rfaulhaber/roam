# bytecode

Bytecode is an intermediary instruction set used in [[programming language]] implementation. Usually used by dynamic languages, a language is [[compiled]] into bytecode before being executed by a [[virtual machine]].

One might imagine a simple bytecode with three operations: increment the single register, decrement the single register, and quit:

```text
INC = 0
DEC = 1
QUIT = 2
```

The following bytecode would increment the register three times, decrememnt once, and quit:

```text
00012
```

The benefit of this approach is twofold: it&rsquo;s portable, and it allows for faster interpretation. A [[Lisp]] or Python benefits from this.

Bytecode is used in more than one [[way to run code]].

