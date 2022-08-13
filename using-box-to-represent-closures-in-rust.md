# using Box to represent closures in Rust

tags
: [[Rust]] [[programming]] [[functional programming]]

Due to Rust&rsquo;s static typing, sometimes it can be difficult to represent things that come easier in other languages. While trying to implement [[Dial]], this issue came up again and again.

I want to be able to use closures on the fly to reduce the burden of evaluation. One possible way that this could be done is the following:

```rust
use std::collections::HashMap;

enum MyType {
    Fn(Box<dyn Fn(i64) -> i64>),
}

struct Env {
    map: HashMap<&'static str, MyType>,
}

fn main() {
    let mut my_map = Env {
        map: HashMap::new(),
    };

    my_map.map.insert("add10", MyType::Fn(Box::new(|x| x + 10)));
    my_map.map.insert("sub10", MyType::Fn(Box::new(|x| x - 10)));

    match my_map.map.get("add10") {
        Some(MyType::Fn(f)) => println!("{}", f(20)),
        _ => unreachable!(),
    };

    match my_map.map.get("sub10") {
        Some(MyType::Fn(f)) => println!("{}", f(20)),
        _ => unreachable!(),
    };
}
```

