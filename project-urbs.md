# Project Urbs

tags
: [[my projects]]

Project Urbs is the codename for a personal project I&rsquo;d like to work on. This is the space to work out the details of Urbs.

_Urbs_ is Latin for &ldquo;city.&rdquo;

~~Project Urbs will be a [[Tumblr]] clone of sorts, except that each post is essentially its own application, similar to [[HyperCard]].~~

This is no longer true, the above has been split into [[postwave]]. Urbs will be purely about application development, like a cross between Glitch and Itch.io.


## Design


### Desired features

-   All posts are cards
-   Cards can be as simple as rich text and as complicated as an application
-   Cards are programmable with UrbsTalk
-   Cards need to be interactive
-   Must allow for drag-and-drop interface


### Tech stack


#### Server

Written in Rust, using:

-   Sqlx
-   Actix-web


#### Front-end

Written in TypeScript:

-   [SolidJS](https://solidjs.com/)
-   Parcel 2 for bundling


##### Client library

TypeScript REST and WebSocket API


#### [[UrbsTalk]] interpreter

-   Rust-compiled WASM (perhaps use Parsel to import to front-end)


#### Drag and drop interface

-   Research will be done with [[webtools]]


#### Database

-   PostgreSQL


#### Devops

-   Ansible to set up server
-   Docker for every part of application


## Possible names


### &ldquo;vcity&rdquo;

Pronounced like &ldquo;vee-sity&rdquo; or &ldquo;vis-it-ee&rdquo;.


### &ldquo;blogic&rdquo;

Seems to already be a minor brand. I like the use of &ldquo;logic&rdquo; though.


### &ldquo;udana&rdquo; or some variation

Take &ldquo;Xanadu&rdquo;, reverse it, and nix the &ldquo;X&rdquo;
