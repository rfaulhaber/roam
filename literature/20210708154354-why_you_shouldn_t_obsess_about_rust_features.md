# Why you shouldn't obsess about Rust "features"

source
: [Why you shouldn&rsquo;t obsess about Rust &ldquo;features&rdquo; | NullDeref](https://nullderef.com/blog/rust-features/)


<a id="orgc0e7a19"></a>

## Notes

[[Rust]] features allow for conditional compilation.

The author managed to not need features, and instead was able to write a `Config` struct.

Features are a big deal and you should avoid using them unless you absolutely need conditional compilation.
