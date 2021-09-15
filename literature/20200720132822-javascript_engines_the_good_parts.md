# JavaScript Engines: The Good Parts

tags
: [[JavaScript]]

[source](https://www.youtube.com/watch?v=5nmpokoRaZI)


<a id="orgc3775da"></a>

## Notes

-   Steps for JavaScript interpretation:
    1.  Source code -> parser -> AST
    2.  AST -> interpreter -> bytecode
    3.  interpreter -> optimizer -> optimized code
-   V8 interpreter takes profiling data on &ldquo;hot&rdquo; functions, optimizes that code
-   SpiderMonkey partially optimizes code and then further optimizes code based on hot code
