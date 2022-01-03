# Parry language design

tags
: [[programming languages]] [[programming language implementation]] [[my programming languages]] [[language design]]

Parry is a programming language I&rsquo;d like to implement. Parry is inspired by [Riposte](https://docs.racket-lang.org/riposte/).


## Design

Parry programs are centered around making REST requests. The goal of Parry is to make REST requests scriptable. Parry should make it trivial to, say, do a `GET` request from one resource and then use that information in subsequent requests.

```text
url_root = "api.example.com"

resource = get "${url_root}/foo"
another_resource = get "${url_root}/bar?id=${resource.id}"

let
    url = "${url_root}/baz"
    headers = {
        Content-Type: "application/json",
    }
    body = {
        parent_id: another_resource.id
    }
in
    post url headers body
```


## Types

Parry is a loosely typed language and supports many basic types.


### Primitives


#### Booleans

```text
bool = true || false
```


#### Numbers

Parry supports integers and floating point numbers.

```text
v = 1 + 3.22 - -1 * -3.45
```


#### Strings

```text
str = "hello world"
strSingle = 'single quotes are okay too'
```

String interpolation can occur in any string.

```text
strInterp = "${str} foo bar"
```


### Lists

Lists can have mixed types and do not require commas.

```text
[1 2 "foo" true]
```


### Sets

Sets are a simplified kind of JSON. They are also immutable.

```text
obj = {
    foo: 123,
    bar: 'bar',
    baz: false,
    quux: [1 2 3 4],
    quuz: {
        foo: 'foo'
    },
}
```

Set properties are accessed with dots.

```text
foo = obj.foo
bar = obj?.bar # returns `null` if not present
```

To derive a new set, you can use the spread operator.

```text
obj2 = {
    ...obj1,
    foo: 456 # overrides `foo`
}
```


### Functions

All functions are first class, lambdas, and anonymous.

```text
myFunc = (bar, baz) -> bar + baz;
```


## Operators

| Operator | Description           |
|----------|-----------------------|
| `+`      | Addition              |
| `-`      | Subtraction           |
| `*`      | Multiplication        |
| `/`      | Division              |
| `<`      | Greater than          |
| `<=`     | Greater than or equal |
| `>`      | Less than             |
| `>=`     | Less than or equal    |
| `.`      | Set access            |
| `?.`     | Optional set access   |
| `=`      | Assignment            |
| `>>`     | Write to file         |
| `<<`     | Read file to value    |


## Control Flow


### let &#x2026; in

```text
let
    a = 1
    b = 2
    c = 3
in
    a + b + c
```


### Conditionals

Conditionals are expressions.

```text
a = if isEven(4) then true else false
```


### match

```text
a = match b
    | "foo" -> 1
    | "bar" -> 2
    | else -> 3
```


### Loops


## File IO

Both reading and writing return `null`.


### Reading

```text
txt << "~/file.txt"
```


### Writing

```text
"foo bar" >> "~/out.txt"
```


## REST requests

Making REST requests are at the center of Parry.

All REST requests are functions that either take a single string or a set. If only a string is specified, say

```text
get "api.example.com/foo"
```

The request made will simply be a `GET` to that URL, with no further information.

However, you could also specify a set, as such:

```text
get {
    url: "api.example.com/foo",
    headers: {
        Accept: "*/*"
    }
}
```

By passing a set you can specify header and body information.


### `get`


### `post`


## Standard Library


### Map

```text
map([1 2 3], (v) -> "${v}") # => ["1" "2" "3"]
```


### Reduce

```text
reduce([1 2 3], (sum, v) -> sum + v, 0) # => 6
```


### Assert
