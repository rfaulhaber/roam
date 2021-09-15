# Kroz language design

tags
: [[my programming languages]]

Kroz is a programming language project I&rsquo;m using to better familiarize myself with [[Racket]] and implementing languages in Racket.

Kroz is a language that&rsquo;s used for designing text-based adventure games. The output of a Kroz program is a full-blown adventure game.


<a id="org58b0787"></a>

## Design

The language consists of a series of prompts and expected responses. A Kroz game is a tree of decisions.

```text
; this is a comment
game_start:
    prompt "You find yourself in a dungeon cell with only a rope. What do you do?"
    actions:
        "climb rope" -> climb_rope_action
        "yell" -> end_game "A guard comes and kills you."

climb_rope_action:
    prompt:
    ---
    This is how you'd write a multi-line prompt.
    ---
    actions:
        "follow road" -> ...

begin_game game_start
```


<a id="org9226b83"></a>

### Scenes

Every program is broken into scenes. Each scene has an identifier specified with it:

```text
game_start:
...
```

Scenes have a `prompt` and `action`.


<a id="org7bbd2c4"></a>

### Prompt

The prompt is the text that is displayed at the beginning of a scene. Prompts can be strings surrounded by quotes, such as:

```text
prompt: "You enter a cave."
```

or multi-line string literals surrounded by `---`. The first `---` must begin on a new line.

```text
prompt:
---
This is a raw string.

This will be displayed to the user exactly as you type it!
---
```


<a id="orgb2fdb8b"></a>

### Actions

Actions are essentially key-value mappings from accepted inputs to other scenes, delimited by new lines. Their basic syntax is:

```text
ACTION = WORD_LIST -> SCENE_NAME \n
WORD_LIST = WORD+
WORD = ".*"
```

For example:

```text
actions:
"follow road", "grab rope" -> next_scene_name
"cut rope"                 -> different_scene
```

Whitespace is ignored on a line-by-line basis.


<a id="orgfe3cafe"></a>

### Special actions

There are two special reserved actions, `begin_game` and `end_game`.


<a id="orgf3420d7"></a>

#### `begin_game`

`begin_game` tells the program which scene kicks the game off. Conventionally it should be placed at the end of a Kroz file.

```text
begin_game first_scene
```


<a id="org92fcd09"></a>

#### `end_game`

`end_game` ends the game. It takes no argument and can be used in an action list.

```text
actions:
"foo" -> end_game
```
