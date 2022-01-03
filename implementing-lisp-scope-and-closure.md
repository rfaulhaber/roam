# implementing Lisp scope and closure

tags
: [[Lisp]] [[Dial]] [[Dial implementation]] [[Rust]]

In implementing a Lisp, there is the problem of scope and closure.


## Memory efficiency

Since we are making a programming language, we&rsquo;d want our language&rsquo;s interpreter to be as efficient as possible[^fn:1]. Therefore we should try to reduce cloning data as much as possible. This could be achieved with the use of `RefCell` in Rust.


## Scopes

How do we deal with scoping in general? Consider the following:

```lisp
(def x (fn (a) + a 10)
```

How do we implement this without `a` conflicting with something at a higher level?


### Possible solutions


<a id="org5b4272d"></a>

#### Recursive `env`

MAL and Risp define an env as such, essentially:

```rust
struct Env {
    bindings: HashMap<String, Val>
    outer: Option<Env>,
}
```

There are a couple of concerns I have with this. Once you start introducing lifetimes, this becomes quite complicated.


#### Flat table

[[Lisp in Small Pieces]] implies that one way to store variables would be to store them all in the same place as one collection. This would perhaps make lookup more complicated.

```rust
struct Env {
    bindings: HashMap<String, Binding>
}

struct Binding {
    val: Val,
    scope: usize, //                        (scope)
}
```

Here scope would be a reference to something that provides information about how it could be accessed. This might make memory management simpler but would make lookup complicated, as each scope would need to know:

-   its own ID
-   its parent ID
-   if its a valid reference
-   what values shadow it

These problems can be sidestepped with Recursive =env=.


## Tail-call problem

If our interpreter is rewritten to essentially be one big `while` loop, scope and garbage collection becomes a problem.

At the end of each iteration of the loop, we need to clear any values that should go out of scope. For example, if our `eval` function evaluates the following:

```lisp
((lambda (a b) + a b) 1 2)
```

We would need `a` and `b` to not live beyond the `lambda` and have the `lambda` not persist after its evaluation.
