# JavaScript Engines: The Good Parts

tags
: [[JavaScript]]

[source](https://www.youtube.com/watch?v=5nmpokoRaZI)


## Notes

-   Steps for JavaScript interpretation:
    1.  Source code -&gt; parser -&gt; AST
    2.  AST -&gt; interpreter -&gt; bytecode
    3.  interpreter -&gt; optimizer -&gt; optimized code
-   V8 interpreter takes profiling data on &ldquo;hot&rdquo; functions, optimizes that code
-   SpiderMonkey partially optimizes code and then further optimizes code based on hot code

