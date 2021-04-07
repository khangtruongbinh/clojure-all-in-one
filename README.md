# Chapter 3: Do things: A clojure crash course
## `Syntax`
Clojure's syntax is simple. Like all Lisps, it employs a uniform structure, a handful of special operators, and a constant supply of parentheses delivered from the parenthesis mines hidden beneath the MIT, where Lisp was born

### `Forms`
All Clojure code is written in a uniform structure. Clojure recognize two kinds of structures:

- Literal representations of data structures (like numbers, strings, maps and vectors)
- Operations

Term `form` is usde to refer to valid code. Sometimes, `expression` is used. Clojure `evaluate` every from to produce a value. These literal representations are all valid forms:
```clojure
1
"a string"
```
Your code will rarely contain free-floating literals, of course, because they don't actually do anything on their own. Instead, you'll use literals in operations. Operations are how you do things. All operations take the form `opening parenthesis`, `operator`, `operands`, `closing parenthesis`:
```
(operator operand1 operand2 operand3 ... operandn)
```
Notice that there are no commas. Clojure uses whitespace to separate operands, and it treats commas as whitespace. Here are some example operations:
```clojure
(+ 1 2 3)
; => 6
```
```clojure
(str "lorem " "ipsum")
; => "lorem ipsum"
```

### `Control flow`
There are three basic control flow operators: `if`, `do` and `when`. Throughout the book you'll encounter more, but these will get you started.
#### `if `
Thif is the general structure for an `if` expression:

```clojure
(if boolean-form
    then-form
    optional-else-form)
```
A Boolean form is just a form that evauluates to a truthy or falsey value. Here examples:
```clojure
(if true
    "By Zeus's hammer!"
    "By Aquaman's trident!")
; => "By Zeus's hammer!"
```
You can also omit the `else` branch. If you do that and the Boolean expression is false, Clojure returns nil, like this:
```clojure
(if false)
    "Just OK")
; => nil
```
Notive that `if` uses operand position to associate operands with the `then` and `else` branches: the first operand is the `then` branch, and the second operand is the (optional) `else` branch. As a result, each branch can have only one form.
#### `do`
The `do` operator lets you `wrap up` multiple forms in parentheses and run each of them. Try the following in your REPL:
```clojure
(if true
    (do (println "Success"!)
        "By Zeus's hammer!")
    (do (println "Failure")
        "By Aquaman's trident!"))
; => Success!
; => "By Zeus's hammer!"
```
This operator lets you do multiple things in each of the `if` expression's branches. In this case, two things happen: `Success!` is printed in th REPL, and `"By Zeus's hammer!"` is returned as the value of the value of the entire if expression.
#### when
The `when` operator if like a combination of `if` and `do`, but with no `else` branch. Here's an example:
```clojure
(when true
    (println "Success!")
    "abra cadabra")
; => Success!
; => "abra cadabra"
```
Use `when` if want to do multiple things when some condition is true,
and you always want to return `nil` when the condition is false.
#### `nil, true, false, Truthiness, Equality and Boolean Expressions`
Clojure has `true` and `false` values, `nil` is used to indicate *no value* in Clojure. You can check if a value is `nil` with the appropriately named `nil?` function:

```clojure
(nil? 1)
; => false
```
Both `nil` and `false` are used to represent logical falsiness, wherereas all other values are logically truthy. *Truthy* and *falsy* refer to how a value is treated in a Boolean expression, like the first expression passed to `if`:
```clojure
(if "bears eat beets"
    "bears beets Battlescar Galatica")
; => "bears beets Battlescar Galatica"

(if nil
    "This won't be the result because nil is fasley"
    "nil is falsy")
; => "nil is falsy"
```
In the first example, the string `"bears eat beet"` is considered truthy, so the `if`, expression evaluates to `"bears beets Battlescar Galatica"`. The second example shows a falsy value as falsey:
Clojure's equality operator is `=` :
```clojure
(= 1 1)
; => true

(= nil nil)
; => true

(= nil false)
; => false
```
Some other languages require you to use different operators when comparing values of different types. For example, you might have to use some kind of special string equality operator mode just for strings. But you don't need anything weird or tedious like test for equality when using Clojure's built-in data structures.