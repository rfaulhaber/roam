# Creating universal Emacs Lisp packages and scripts

source
: [Creating universal Emacs Lisp packages and scripts | DanPetrov](https://danpetrov.xyz/emacs/lisp/programming/2022/01/15/creating-universal-emacs-lisp-packages.html)


## Notes

You can add a [[shebang]] to the top of an [[Emacs Lisp]] script to have it evaluated in batch mode:

```emacs-lisp
#!/usr/bin/env emacs --script
```

Secondly, you can determine if a script is running in batch mode with the following predicate:

```emacs-lisp
(defun foo-running-as-script-p ()
  "Return truthy if running as Elisp script."
  (member "-scriptload" command-line-args))
```

Finally, it&rsquo;s worth adding some kind of &ldquo;main&rdquo; function:

```emacs-lisp
(defun main ()
  "Entrypoint for foo"
  (pprint command-line-args-left)
  (message "Do stuff here"))

(when (foo-running-as-script-p)
  (main))
```
