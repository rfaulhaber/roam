# Type-Driven Development With Idris

source
: 

Note: code for this book in [[Idris2]] can be found [here](https://github.com/edwinb/Idris2/tree/master/tests/typedd-book).


<a id="org9f9fee8"></a>

## 1. Overview

[[Type-driven development]] could be thought of as designing the [[types]] for a program first, and understanding a program as the interaction of those types.

The concept of types outlined in this chapter sounds very similar to [[category theory]].

Type-driven development can be used in [[concurrent programming]] by defining an interface that describes the form of messages it will handle and define a protocol describing how the messages will be sent and in what order.

Type-driven development is driven by the following process:

1.  Type. Write a type and use it for relevant functions.
2.  Define. Write a function that satisfies input and output types using the above type.
3.  Refine. Exclude invalid input and output.

Types can be dependent, i.e. is calculated by some other values. Consider a hypothetical `append` function:

|           | Input 1 type | Input 2 type | Output type       |
|-----------|--------------|--------------|-------------------|
| Simple    | AnyList      | AnyList      | AnyList           |
| Generic   | List elem    | List elem    | List elem         |
| Dependent | Vect n elem  | Vect m elem  | Vect (n + m) elem |

[[Idris]] is a [[pure functional programming]] language.

[[functional programming]]
: programs are composed of first class functional constructs, and program execution consists of evaluating those functions

[[pure functional programming]]
: in addition to functional programming, functions do not have [[side-effects]] and all functions are [[idempotency]]

Although a pure functional programming language cannot perform side-effects, it can describe them.

[[referential transparency]]
: the property of a function to produce the same output as an input. A key to [[pure functional programming]]

In [[Idris]], a [[type]] that describes a [[side-effect]] is denoted as such, e.g. `String` vs. `IO String`.

[[total function]]
: a function that always returns a result

[[partial function]]
: a function that may not return a result given some inputs, i.e. the output is not defined


<a id="org6d2c575"></a>

## 2. Getting started with Idris

Listing 2.1 code reproduced here for reference:

```idris
module Main

average : (str : String) -> Double
average str = let numWords = wordCount str
                  totalLength = sum (allLengths (words str)) in
                  cast totalLength / cast numWords
    where
      wordCount : String -> Nat
      wordCount str = length (words str)
      allLengths : List String -> List Nat
      allLengths strs = map length strs

showAverage : String -> String
showAverage str = "The avareage word length is "  ++ show (average str) ++ "\n"

main : IO ()
main = repl "Enter a string: " showAverage
```


<a id="org8b5036d"></a>

### Types


<a id="orgb10e11c"></a>

#### Numbers

Int
: _fixed-width_ integer type

Integer
: _unbounded_ signed integer type

Nat
: _unbounded_ unsigned integer type[^fn:1]

Double
: double-precision floating-point type

Idris treats numbers as `Integer` by default.

Idris provides a `cast` function to convert between types.


<a id="orgf34d7c1"></a>

#### Characters and strings

Char
: character, enclosed in single quotes

String
: string literal, enclosed in double quotes


<a id="org5903deb"></a>

#### Booleans

-   `True` and `False`

The inequality operator is `/=`, which comes from [[Haskell]].


<a id="orgdddf2be"></a>

### Functions

Functions have types.

```idris
double : Int -> Int
double x = x + x
```


<a id="org7c5f820"></a>

#### Partial application

Idris supports [[partial application]] by default.

```idris
add : Int -> Int -> Int
add x y = x + y

add 2 3
-- 5 : Int, expected behavior

add 2
-- add 2 : Int -> Int
```


<a id="org8293851"></a>

#### Generic functions

```idris
identity : ty -> ty
identity x = x
```


<a id="orga651766"></a>

#### Generic functions with constrainted types

Consider,

```idris
doulbe : ty -> ty
double x = x + x
```

This would result in an error because it&rsquo;s not constrained to a number type. Instead, we want:

```idris
doulbe: Num ty => ty -> ty
double x = x + x
```

`Num ty` means that `ty` is constrained by `Num` types. `Num` is an interface, which will be covered later.

Infix number operators, like in Haskell, are actually functions:

```idris
lessThanThree : Bool
lessThanThree = (< 3)
```


<a id="orgc2bc273"></a>

#### Anonymous functions

```idris
let anonymous = (\x => x * x) 2
let anonymousWithTypes = \x : Int, y : Int => x + y
```


<a id="org8705fd6"></a>

#### `Let` and `where`

`let ... in` creates a local binding in an expression, e.g. `let x = 50 in x + x`.

`where` contain local binding definitions.

```idris
pythagoras : Double -> Double -> Double
pythagoras x y = sqrt (sqaure x + square y)
where
  square : Double -> DOuble
  square x = x * x
```


<a id="orged243a0"></a>

### Composite types


<a id="orgdf31f80"></a>

#### Tuples

Fixed sized collections.

```nil
(92, "Pages")
```


<a id="org6be1660"></a>

#### Lists

Any-size collection, but must be same type.

```nil
[1, 2, 3, 4]
```

1.  List operators

    ```nil
    [1, 2] ++ [3, 4] -- [1, 2, 3, 4]
    1 :: [2, 3, 4] -- [1, 2, 3, 4], called "cons"
    ```


<a id="org9aaf3f5"></a>

## 3. Interactive development with Types
