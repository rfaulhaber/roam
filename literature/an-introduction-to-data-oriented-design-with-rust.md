# An introduction to Data Oriented Design with Rust

source
: [An introduction to Data Oriented Design with Rust - Statistically Insignificant](https://jamesmcm.github.io/blog/2020/07/25/intro-dod)

tags
: [[Rust]]


## Notes

[[Data-oriented design]] is a programming paradigm whereby the layout of data structures is strongly considered for performance reasons.

In a [[object-oriented]] approach, the code for a player might look like this:

```rust
pub struct Player {
    name: String,
    health: f64,
    location: (f64, f64),
    velocity: (f64, f64),
    acceleration: (f64, f64),
}
```

whereas in DOD, it would look like this:

```rust
pub struct DOPlayers {
    names: Vec<String>,
    health: Vec<f64>,
    locations: Vec<(f64, f64)>,
    velocities: Vec<(f64, f64)>,
    acceleration: Vec<(f64, f64)>,
}
```

This would increase performance dramatically because we&rsquo;re better using the CPU cache. The article has some good statistics on this.
