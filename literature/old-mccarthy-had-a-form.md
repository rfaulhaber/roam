# Old McCarthy Had a Form

source
: [Old McCarthy Had a Form - Atomized](http://atomized.org/blog/2021/11/28/old-mccarthy-had-a-form/)


## Notes

[[EIEIO]] is an implementation of [[CLOS]] for [[Emacs]].

[[Classes]] in EIEIO/CLOS _only_ encapsulate data, they do not have methods.

```elisp
(defclass form/emms-player ()             ; parent class/es
  ((playing :type boolean :initform nil)) ; slot (field) defintions
  :abstract t)                            ; EIEIO extension
```

[[EIEIO]] has [[abstract classes]], like [[Java]], and an EIEIO abstract class can be extended but not instantiated by itself.

The article provides an example extension as such:

```elisp
(defclass form/emms-player-mpv (form/emms-player) ; Child of form/emms-player
  ())                                             ; no additional slots
```

Instantiating objects involves calling their constructor.

```elisp
(form/emms-player-mpv) ; constructor

(let ((class 'form/emms-player-mpv))
  (make-instance class)) ; instantiation based on a symbol value

(list #s(form/emms-player-mpv nil) ; use of the #s macro, which evaluates to an equivalent object
      (read "#s(form/emms-player-mpv nil)"))
```

To access slots within a class, you can use the `oref` or `oset` macros.

```elisp
(let ((p (form/emms-player-mpv)))
  (progn (oset p playing t)
	 (oref p playing)))
```

You can also reference slots by symbol:

```elisp
(let ((slot 'playing))
  (slot-value (form/emms-player-mpv) slot)) ; gets the ~playing~ slot
```

There is also the `with-slots` macro:

```elisp
(with-slots (playing) (form/emms-player-mpv)
  (list playing (progn (setf playing t) playing)))
```

This is a bit like [[destructuring]].

A class can provide an `:initform` keyword, which is like the default constructor. It says how the class is initialized.

```emacs-lisp
(defclass form/now ()
  ((time :initform (current-time-string))))

(list
 (oref (form/now) time)
 (progn (sleep-for 1)
	(oref (form/now) time)))
```

You may also specify an `:initarg` form for a field. This allows a constructor to specify slot values on initialization, as such:

```emacs-lisp
(defclass foo ()
  ((val :initarg :val)))

(let ((obj (foo :val 1)))
  (oref obj val))
```

The rough equivalent in Java might be:

```java
public class Foo {
    private int val;

    public Foo(val) {
        this.val = val;
    }
}

// elsewhere
Foo f = new Foo(1);
```

CLOS specifies something called [[generic functions]]. These are similar to [[interfaces]], in that they only specify a name and an argument list. In CLOS, [[methods]] are simply implementations of generic functions.

CLOS (and EIEIO) allow for method implementation. Given generic functions, as such:

```common-lisp
(cl-defgeneric form/emms-playablep (player track))
(cl-defgeneric form/emms-start     (player track))
(cl-defgeneric form/emms-stop      (player t))
```

You could provide implementations for these functions as follows:

```common-lisp
;; Method applies when PLAYER arg is an instance of form/emms-player-mpv (or a subclass)
(cl-defmethod form/emms-playablep ((player form/emms-player-mpv) track)
  (not (eql :unplayable track)))       ; MPV can play almost anything!

(cl-defmethod form/emms-start     ((player form/emms-player-mpv) track)
  "Started")

(cl-defmethod form/emms-stop      ((player form/emms-player-mpv))
  "Stopped")
```

CLOS uses qualifiers to specify four different kinds of methods. They are

-   primary methods, which we have already seen
-   `:before`, evaluated before the primary method
-   `:after`, evaluated after the primary method
-   `:around`, evaluated around all other method types. This value can actually modify the return value of a function

This is similar to [[Emacs Lisp]]&rsquo;s [[function advice]].

EIEIO also allows for multiple [[inheritence]], for example:

```emacs-lisp
(defclass form/logger ()
  ((messages :initform nil)))

(cl-defmethod form/log ((logger form/logger) format-string &rest args)
  (with-slots (messages) logger
    (push (apply #'format format-string args) messages)))

(cl-defmethod form/logs ((logger form/logger))
  (with-slots (messages) logger messages))

(cl-defmethod form/latest-log ((logger form/logger))
  (car (form/logs logger)))

(let ((l (form/logger)))
  (form/log l "Hello, %s!" "world")
  (form/latest-log l))
```

EIEIO also allows for defining structs for when you only care about keeping track of organized data. This is done with `cl-defstruct`.
