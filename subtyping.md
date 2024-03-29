# subtyping

Subtyping, at least in programming, is when a [[type]] is somehow related to another type via substitution. A typical example is that `Dog` is a subtype of `Animal`. In some cases, the latter could be substituted for the former, whereas the former could always substitute the latter.

The fancy symbol for subtyping is &ldquo;≼&rdquo;, so &ldquo;A ≼ B&rdquo; means that A is a subtype of B.

Subtyping is transitive, meaning that if A ≼ B ≼ C, then A ≼ C.

Assuming A ≼ B,

covariance
: when (T -&gt; A) ≼ (T -&gt; B)

contravariance
: when (B -&gt; T) ≼ (A -&gt; T)

bivariant
: both covariant and contravariant

In [[TypeScript]] and other strongly typed languages, functions are bivariant.

Generally, you want functions to be covariant in their return type and contravariant in their argument type.


## References

1.  [What are covariance and contravariance? | Stephan Boyer](https://www.stephanboyer.com/post/132/what-are-covariance-and-contravariance)

