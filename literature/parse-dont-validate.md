# Parse, don't validate

source
: [Parse, don’t validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/)


## Notes

When it comes to [[type-driven development]], the right approach should be [[parse, don&rsquo;t validate]].

[[Static type systems]] allow for figuring out what&rsquo;s possible to implement.

Given the [[Haskell]] type signature:

```haskell
head :: [a] -> a
```

You can reason that this would yield a [[partial function]], because it cannot be implemented for `[]`.

However, since we can&rsquo;t implement that type signature fully, we can adjust the signature to be:

```haskell
head :: [a] -> Maybe a
```

indicating that the function may return nothing in certain cases. This may be suitable, but down the road it causes problems, as it forces the rest of the program to deal with a `Maybe` value.

The better solution would be to return a new type, `NonEmpty`, that constrains the type of data the function can work with.

```haskell
data NonEmpty a = a :| [a]

head :: NonEmpty a -> a
head (x:|_) = x
```

Through this process we have **strengthened** our type.

> Consider: what is a [[parser]]? Really, a parser is just a function that consumes less-structured input and produces more-structured output. By its very nature, a parser is a partial function—some values in the domain do not correspond to any value in the range—so all parsers must have some notion of failure. Often, the input to a parser is text, but this is by no means a requirement, and parseNonEmpty is a perfectly cromulent parser: it parses lists into non-empty lists, signaling failure by terminating the program with an error message.

Parsing with a static type system should therefore be simple: &ldquo;if the parsing and processing logic go out of sync, the program will fail to even compile.&rdquo;

[[Shotgun parsing]]
: an [[antipattern]] whereby parsing and input-validating code is mixed with and spread across processing code—throwing a cloud of checks at the input, and hoping, without any systematic justification, that one or another would catch all the &ldquo;bad&rdquo; cases

In practice, parsing without validation looks like focusing on [[datatypes]]. Specifically:

1.  Use data structures that make illegal states unrepresentable.
2.  Push the burden of proof upward as far as possible, but no further.

Alternatively, &ldquo;write functions on the data representation you wish you had, not the data representation that you are given.&rdquo;
