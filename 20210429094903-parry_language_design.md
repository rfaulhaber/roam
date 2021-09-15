# Parry language design

tags
: [[programming languages]] [[programming language implementation]] [[my programming languages]] [[language design]]

Parry is a programming language I&rsquo;d like to implement. Parry is inspired by [Riposte](https://docs.racket-lang.org/riposte/).


<a id="orgc86d245"></a>

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


<a id="orgf02ca79"></a>

## Types

Parry is a loosely typed language and supports many basic types.


<a id="org30a04ac"></a>

### Primitives


<a id="org916731d"></a>

#### Booleans

```text
bool = true || false
```


<a id="orgacde8c3"></a>

#### Numbers

Parry supports integers and floating point numbers.

```text
v = 1 + 3.22 - -1 * -3.45
```


<a id="orgd11f6d5"></a>

#### Strings

```text
str = "hello world"
strSingle = 'single quotes are okay too'
```

String interpolation can occur in any string.

```text
strInterp = "${str} foo bar"
```


<a id="org8a2acdd"></a>

### Lists

Lists can have mixed types and do not require commas.

```text
[1 2 "foo" true]
```


<a id="orgbc8b16d"></a>

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


<a id="org855d9d8"></a>

### Functions

All functions are first class, lambdas, and anonymous.

```text
myFunc = (bar, baz) -> bar + baz;
```


<a id="orgf45f5fc"></a>

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


<a id="orgcdb3a7f"></a>

## Control Flow


<a id="orge90698b"></a>

### let &#x2026; in

```text
let
    a = 1
    b = 2
    c = 3
in
    a + b + c
```


<a id="org6867ce5"></a>

### Conditionals

Conditionals are expressions.

```text
a = if isEven(4) then true else false
```


<a id="orgc214d02"></a>

### match

```text
a = match b
    | "foo" -> 1
    | "bar" -> 2
    | else -> 3
```


<a id="org574a26d"></a>

### Loops


<a id="org8c07faa"></a>

## File IO

Both reading and writing return `null`.


<a id="org640bb83"></a>

### Reading

```text
txt << "~/file.txt"
```


<a id="org28ac08d"></a>

### Writing

```text
"foo bar" >> "~/out.txt"
```


<a id="org2fe9546"></a>

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


<a id="org82b79a4"></a>

### `get`


<a id="orgeedb756"></a>

### `post`


<a id="orgf093748"></a>

## Standard Library


<a id="org025ca46"></a>

### Map

```text
map([1 2 3], (v) -> "${v}") # => ["1" "2" "3"]
```


<a id="orgc41671f"></a>

### Reduce

```text
reduce([1 2 3], (sum, v) -> sum + v, 0) # => 6
```


<a id="org83152e6"></a>

### Assert
