## An Introduction to Lisp

### by Yao Li
;;;;
## What is Lisp?

- Lisp is short for **LIS**t **P**rocessor
- Lisp is a family of computer programming languages
- Lisp is the second-oldest high-level programming language (It's created in 1958. Only Fortran is older).
- Lisp was originally created as a practical mathematical notation for computer programs, influenced by the notation of Church's lambda calculus.

<small>* From [Wikipedia](http://en.wikipedia.org/wiki/Lisp_(programming_language&#41;)</small>

;;;;
## Lisp is Different
;;
## Lisp is Different
### Most Pressed Keys
<div class="pic">![C++](images/keys_cpp.png)</div>
###### C++
;;
## Lisp is Different
### Most Pressed Keys
<div class="pic">![Python](images/keys_python.png)</div>
###### Python
;;
## Lisp is Different
### Most Pressed Keys
<div class="pic">![Lisp](images/keys_lisp.png)</div>
###### Common Lisp
;;;;
## Lisp Family
- Common Lisp
- Scheme
- Emacs Lisp
- Clojure

<p class="fragment">Let's start with Common Lisp.</p>
;;;;
## Content

- Hello World & REPL
- Common Lisp Syntax
- Understanding Lists
- Macros

;;;;
## Hello World & REPL
### Demonstration
;;;;
## Common Lisp Syntax
Common Lisp code is written in s-expression:

    (format t "Hello World!")

<p class="fragment">The basic elements of s-expressions are lists and atoms.</p>
;;
## Lists and Atoms
Lists are delimited by parenthese and can contain any number of whitespace-separated elements.

    (format t "Hello World!")
    (1 1 2 3 5)

Atoms are everything else.

    "Hello World!"
    1

`NIL` or `()`, which stands for empty list, is both list and atom.

<small>* Definitions are from [Practical Common Lisp](http://gigamonkeys.com/book/syntax-and-semantics.html)</small>
;;
## Lists: Homoiconicity

    (1 1 2 3 5)
    ("foo" "bar")
    (x 1 "foo")
    (+ (* 2 3) 4 5)
    (defun hello-world ()
      (format t "hello, world"))

;;

## Lists in Common Lisp

There are four kinds of lists in Common Lisp:

- Data,
- Function calls,
- Special operators,
- Macros.

;;;;
## Understanding Lists

What's the structure of a list?

A list is made up of cells:

    (cons 5 NIL)  ; (5)
    (cons 1 (cons 1 (cons 2 (cons 3 NIL))))  ; (1 1 2 3)
    (cons 1 2)  ; (1 . 2)
    
;;
## An Example

For a list:

    (1 1 2 3)  ; (cons 1 (cons 1 (cons 2 (cons 3 NIL))))

The structure is like:

    *--*--*--*--nil
    |  |  |  |
    1  1  2  3

;;
## Another Example

Think: what would the structure be for a list like:

    (cons (cons 1 2) (cons 3 4))

;;
## Another Example

Think: what would the structure be for a list like:

    (cons (cons 1 2) (cons 3 4))

The structure is like:

       *
      / \
     *   *
    / \ / \
    1 2 3 4

<p class="fragment">Yes. It's a binary tree!</p>

;;
## Using Lists

### Demonstration

<center>Let's implement a `range` function like in Python.</center>


;;
## Using Lists

The function should be like this:

    (defun range (start end)
      (range-iter start end NIL))
    (defun range-iter (start end lst)
      (if (>= start end) (nreverse lst)
          (range-iter (1+ start) end (cons start lst))))

Lists work quite well with recursive functions.

;; Actually...

The `range` function can also be defined in this way:

    (defun range (start end)
      (loop for i from start below end collect i))

What is the `loop` doing here?

<p class="fragment">Well, it's a macro.</p>

;;;;
## Macros

<blockquote>
The first rule of Macro Club is Don't Write Marcros.
</blockquote>
<small>* From <em>Programming Clojure</em>, by Stuart Halloway</small>

;;
## Swap

In programming languages like C, macros are just text substitutions.

For example, let's write a `swap` macro in C.

    #define swap(x, y) { int tmp = x; x = y; y = tmp; }

It seems fine. But what would happen if you call:

    swap(a, tmp);

or

    swap(a[i++], a[j])

;;
## Swap

In Common Lisp, you can define the macro in this way:

    (defmacro swap (a b)
      (let ((tmp (gensym)))
        `(progn
          (setf ,tmp ,a)
          (setf ,a ,b)
          (setf ,b ,tmp))))

`gensym` will automatically generate a unique name.

<p class="fragment">However, you don't really need this in Common Lisp. Use the built-in <code>rotatef</code> instead.</p>

;;
## By The Way...

In Scala (since 2.10.0), you write this...

    object Whatever {
      def swap(a: Any, b: Any): Unit = macro swap_impl

      def swap_impl(c: Context)(a: c.Expr[Any], b: c.Expr[Any]): c.Expr[Unit] = {
        import c.universe._
        import c.universe.Flag._

        def assign(l: c.Expr[Any], r: c.Expr[Any]) = {
          l.tree match {
            case il : Ident => Assign(il, r.tree)
            case Apply(Select(obj, sel), List(index)) if sel.toString == "apply" => 
              Apply(Select(obj, newTermName("update")), List(index, r.tree))        
            case Select(obj, sel) => Apply(Select(obj, sel.toString + "_$eq"), List(r.tree))
            case _ => c.abort(l.tree.pos, "Expected variable or variable(index)")
          }
        }

        c.Expr[Unit](Block(
          ValDef(Modifiers(MUTABLE), newTermName("$temp"), TypeTree(), a.tree),
          assign(a, b),
          assign(b, c.Expr[Any](Ident(newTermName("$temp"))))))
      }
    }

;;
## Even More...

A silly example:

    (defmacro backwards (expr) (reverse expr))

Then you can write programs in this way:

    (backwards ("hello world" t format))

What does this mean?


<p class="fragment">You can define your own syntax,</p>
<p class="fragment">like what's happened in <code>loop</code>.</p>

;; 
## Loop

Examples of `loop` usage:

<pre><code data-trim>(loop for i upto 10 collect i) 
(loop for i from 20 downto 10 collect i)
(loop for i in (list 10 20 30 40) by #'cddr collect i) 
(loop for i across "abcd" collect i)
; And much more...
</code></pre>

;;
## However...

<blockquote>With great power comes great responsibility.</blockquote>
<small>* From <em>Spider-Man</em>, 2002, the movie</small>

Lisp programmers can use macros to write short but much more powerful code,
which is very hard for others to understand.

;;;;

## Furthur Reading

##### To learn Common Lisp:

- Practical Common Lisp by Peter Seibel, which can be accessed [online for FREE](http://www.gigamonkeys.com/book/)!
- ANSI Common Lisp by Paul Graham.
- On Lisp by Paul Graham, which contains more advanced topics.

##### Interesting Readings:

- Hackers and Painters by Paul Graham.
- [The Lisp Curse](http://www.winestockwebdesign.com/Essays/Lisp_Curse.html) by Rudolf Winestock.

;;;;
# Thank You!