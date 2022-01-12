# UrbsTalk

tags
: [[my programming languages]]

UrbsTalk is the code name for the custom programming language used in Project Urbs. It is inspired by [[HyperTalk]].

Each UrbsTalk &ldquo;script&rdquo; is contained to a card, and even a component in a card at that.

Each component can have a label. The UI will be similar to Visual Basic Studio.


## Goals

-   Easy to write, idiomatic
-   Should require little technical skill
-   Should be able to manipulate card applications


## Features

-   Statements are line-based


## Possible samples

```text
set dog to "fido"
set radius to 60
draw circle with radius of $radius

on click:
    add 1 to radius
    set radius to 1 - radius
```
