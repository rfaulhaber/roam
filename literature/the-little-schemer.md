# The Little Schemer

tags
: [[lisp]] [[functional programming]] [[Scheme]]

Some of my examples are written in Emacs Lisp, as at the time I hadn&rsquo;t installed a Scheme interpreter. It doesn&rsquo;t make a huge difference.


## 1. Toys

-   an atom is a simple element, separated by spaces
-   a list is a series of atoms, surrounded by parentheses
-   an atom or a list
-   `car` retrieves first element of list
    
    ```emacs-lisp
      (car '(123))
    ```

-   `cdr` retrieves everything besides the first element of a list
    
    ```emacs-lisp
      (cdr '(a b c))
    ```

-   `cons` adds an atom to the front of a list
    
    ```emacs-lisp
      (cons 100 '(a b c))
      (cons '(a b) '(c d))
    ```

-   `null` tests for empty lists
    
    ```emacs-lisp
      (null ())
    ```

-   `atom` tests for atoms
    
    ```emacs-lisp
      (atom 123)
    ```

-   `eq` tests equality for non-numeric atoms
    
    ```emacs-lisp
      (eq 'a 'b)
    ```


## 2. Do It, Do It Again

-   always ask `null?` first!
-   this chapter serves as a good introduction to recursion and tracing through lisp programs


## 3. Cons the Magnificent

-   `cons` is for building lists
-   **important**: when building lists from lists, figure out what the first element looks like, then recur over the `cdr` of the rest of the list

<!--listend-->

```emacs-lisp
(defun firsts (l)
    (cond
    ((null l) '())
    (t (cons (car (car l)) (firsts (cdr l)) ))))

(firsts '((a b) (c d))) ;; => (a c)
```

|   |   |
|---|---|
| a | c |


## 5. **Oh My Gawd**: It&rsquo;s full of Stars

-   In Scheme (and other Lisps?) a \* suffix means &ldquo;repeat this throughout the list.&rdquo; More specifically, it means to recur on the `car` of the list.
    
    For example:
    
    ```scheme
      (rember* 'sauce ((tomato sauce) bean sauce)) ;; => ((tomato) bean)
      (rember 'sauce ((tomato sauce) bean sauce)) ;; => ((tomato) bean sauce)
    ```
-   The first commandment says that, when recurring, you must ask between two to three questions. For a list of atoms, it&rsquo;s &ldquo;`null?`&rdquo; and &ldquo;`else`&rdquo;, and for S-expressions, it&rsquo;s &ldquo;`null?`&rdquo;, &ldquo;`atom? (car l)`&rdquo;, and &ldquo;`else`&rdquo;
-   The fourth commandment says that, when recurring, you must change at least one argument. When traversing over a list, decrement the list using `cdr`
-   The sixth commandment is very important: &ldquo;simplify only after the function is correct&rdquo;


## 6. Shadows

The Seventh Commandment
: Recur on the _subparts_ that are of the same nature

An attempt at `value` (for only `+`) in Emacs Lisp:
    
    ```emacs-lisp
      (defun my-value (nexp)
        (cond
         ((atom nexp) nexp)
         ((eq (car (cdr nexp)) '+) (+ (value (car nexp)) (value (car (cdr (cdr nexp))))))
         (t nexp)))
    
      (my-value '(1 + 2))
    ```

The Eighth Commandment
: Use help functions to abstract from representations
