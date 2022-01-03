# ACT4E - Session 1 - Transmutation

tags
: [[category theory]]

source
: [ACT4E - Session 1 - Transmutation on Vimeo](https://vimeo.com/499578322)


## Notes

-   An arrow describes a transmute, a thing that transforms resources from one to another
-   \\(X \to Y\\) means transforming X into Y
-   Transformations can be **composed**, \\(X \to Y \to Z\\), so for all intents and purposes, \\(X \to Z\\)
-   An (A,B)-process (\\(P(A,B)\\)) consists of:
    -   A set S, element&rsquo;s of which are called _states_
    -   An update function: \\(f: A \times S \to S\\)
    -   A readout function: \\(f: S \to B\\), where, given a certain state, will give you a certain output
-   Given two processes, we can compose a system \\(P(A,C)\\) such that:
    -   \\(A \to P(A,B) \to B \to P(B,C) \to C\\)
-   At a certain abstraction, all these things are the same
-   What makes them the same is composition, transformations, resources, etc.
-   A category \\(C\\) is defined by four constituents:
    -   **Objects:** a collection \\(Ob\_c\\) whose elements are called objects
    -   **Morphisms:** For every pair of objects \\(X, Y \in Ob\_c\\), there is a set \\(Hom\_c(X,Y)\\), the elements of which are called _morphisms_ from X to Y
        -   morphisms are like functions, or &ldquo;arrows&rdquo;
    -   **Identity morphisms:** for each object X, there is an element \\(id\_x \in Hom\_c(X,X)\\) which is called the identity morphism of X
    -   **Composition operations:** given any morphism \\(f \in Hom\_c(X,Y)\\) and any morphism \\(g \in Hom\_c(Y,Z)\\), there exists a morphism \\(f \circ g\\) in \\(Hom\_c(X,Z)\\) which is the _composition_ of f and g
-   Categories must also satisfy the following conditions:
    -   **Unitality:** for any morphism \\(f \in Hom\_c(X,Y): id\_x \circ f = f \circ id\_y\\)
    -   **Associativity:** for \\(f \in Hom\_c(X,Y), g \in Hom\_c(Y,Z), h \in Hom\_c(Z,W): ( f \circ g ) \circ h = f \circ ( g \circ h )\\)
-   Many morphisms can exist between objects
-   Examples:
    -   A currency category could have:
        -   Objects: a collection of currencies
        -   Morphisms: currency exchanges
        -   Identity morphism: 1 USD = 1 USD
        -   Composition of morphisms: Could convert from USD -> CHD -> EURO
-   a subset of a category
-   \\(X -> Y\\) and \\(Y -> X\\) are opposite categories of one another


## Backlinks

-   [[category]]
