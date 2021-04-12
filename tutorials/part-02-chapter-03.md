# Chapter 3: Do things: A clojure crash course
## Syntax
Clojure's syntax is simple. Like all Lisps, it employs a uniform structure, a handful of special operators, and a constant supply of parentheses delivered from the parenthesis mines hidden beneath the MIT, where Lisp was born

### Forms
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

### Control flow
There are three basic control flow operators: `if`, `do` and `when`. Throughout the book you'll encounter more, but these will get you started.
#### if 
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
#### do
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
#### nil, true, false, Truthiness, Equality and Boolean Expressions
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

Clojure uses the Boolean operators `or` and `and`. `or` returns either the first truthy value or the last value. `and` returns the first falsey value `or`, if no values are falsey, the last truthy value. Let's look at `or` first:
```clojure
(or false nil :large_I_mean_venti :why_cant_I_just_say_large)
; => large_I_mean_venti

(or (= 0 1) (= "yes" "no"))
; => false

(or nil)
; => nil
```
In the first example, the return values is `:large_I_mean_venti` because it's the first truthy value. The second example has no truthy values, so `or` returns the last values, which is `false`. In the last example, once again no truth values exists, and `or` returns the last value, which is `nil`. No let's loook at `and`:
```clojure
(and :free_wifi :hot_coffee)
; => :hot_coffee

(and :freelin_super_cool nil false)
; => nil
```
In the first example, and returns the last truthy value, `:hot_coffee`. In the second example, and return `nil`, which is the fisrt falsey value.
### Naming Values with def
You use `def` to bind a name to a value in Clojure:
```clojure
(def failed-protagonist-names
    ["Larry Potter" "Dorren the Explorer" "The Incredible Bulk"])
failed-protagonist-names
; => ["Larry Potter" "Dorren the Explorer" "The Incredible Bulk"]
```
In this case, you're binding the name failed-protagonist-names to a vector containing three strings.

Notice that I'm using the term `bind`, wherereas in other languages you'd say you're `assigning` a value to a `variable`. Those other languages typically encourage you to perform mutiple assignments to the same variable. You might be tempted to do somehting similar in Clojure:
```clojure
(def severity :mild)
(def error-message "Oh f*ck")
(if (= severity :mild)
    (def error-message (str error-message "Mildly inconvinienced"))
    (def error-message (str error-message "Doomed"))
)
```
However, changing the value associated with a name like this can make it harder to understand your program behavior because it's more difficult to know which value is associated with a name or why that value might have changed. Clojure has a set of tools for dealing with change, which you'll learn about in Chapter 10. As you learn Clojure, you'll find that you'll rarely need to alter a name/value association. Here's one way you could write the preceding code:
```clojure
(defn error-message
    [severity]
    (str "Oh god! It's a disaster! We're "
        (if (= severity :mild)
        "mildy inconvenienced"
        "doomed")))
```
Here, you create a function, error-message, which accepts a single argument, *severity*, and uses that to determine which string to return. You then call the function with :mild for the severity. You'll learn all about creating functions in "Function"; in the meantime, you should treat `def` as if it's defining constants. In the next few chapters, you'll learn how to work with this apparent limitation by embracing the functional programming paradigm. 
## Data structures
Clojure comes with a handful of data structures that you'll use the majority of the time. If you're coming from an Object-oriented background, you'll be suprised at how much you can do with seemingly basic types presented here.

All of Clojure's data structures are immutable, meaning you can't change them in place.

Clojure has no equivalent for this. You'll learn more about why Clojure was implemented this way in Chapter 10, but for now it's fun to lean just how to do things without all that philosiophizing. Without further ado, let's look at numbers in Clojure.

### Numbers
Clojure has pretty sophisticated numerial support. I won't spend much time dwelling on the boring technical details (like coercion and contagion). Suffice it to say, Clojure will merrily handle pretty much anything you throw at it.

In the meantime, we'll work with integers and floats. We'll also work with ratios, which Clojure can represent directly. Here's an integer, a float and a ratio, respectively:

```clojure
93
1.2
1/5
```

### Strings

Strings represent text. The name comes from the ancient Phoenicians, who one day invented the alphabet after an accident involving yarn. Here are some example of string literals:
```clojure
"Lord Voldemort"
"\"He who must not be named\""

```
Notice that Clojure only allows double quotes to delineate strings. It only allows concatenation via `str` function:

### Maps

Maps are similar to dictionaries or hashes in other languages. They're a way of associating some value with some other value. The two kinds of maps in Clojure are hash maps and sorted maps. I'll only cover the more basics hash maps. Let's look at some examples of maps literals. Here's an empty map:

```clojure
{}
```
In this example, `:first-name` and `:last-name` are keywords (I'll cover in the next section):
```clojure
{
    :first-name "Charlie"
    :last-name "Macfishwich"
}
```
Here we associate "string-key" with + function
```clojure
{"string-key" +}
```
Maps can be nested:
```clojure
{:name {:first "John" :middle "Stewart" :last "Jungleheimerschidt"}}
```
Notice that map values can be of any type - strings, numbers, maps, vectors, even functions. Clojure don't care!
Besides using map literals, you can use the hash-map function to create a map:
```clojure
(hash-map :a 1 :b 2)
```
You can look up values in maps with the get function:
```clojure
(get {:a 0 :b 1} :b)
; => 1

(get {:a 0 :b {c: "ho hum"}} :b)
; => {:c "ho hum"}
```
In both of these examples, we asked get for the value of the `:b` key in the given map - in the first case it returns `1`, and in the second case it return the nest  maps `{:c "ho hum"}`.

`get` will return `nil` if it doesn't find your key, or you can give it a default value to return , such as `"unicorn?`:

```clojure
(get {:a 0 :b 1} :c)
; => nil

(get {:a 0 :b 1} :c "unicorns?")
; => "unicorns?"
```

The `get-in` funciton lets you look up values in nested maps:
```clojure
(get-in {:a 0 :b {:c "ho hum"}} [:b :c])
; => "ho hum"
```

Another way to look up a value in a map is to treat the map like a function with they key as its argument:
```clojure
({:name "The Human Coffeepot"} :name)
; => "The Human Coffeepot"
```

### Keywords
Clojure keywords are best understood by seeing how they're used. They're primarily used as keys in maps, as you saw in the preceding section. Here are some more examples of keywords:
```clojure
:a
:rumplstiltsken
:34
:_?
```
Keywords can be used as functions that look up the corresponding value in a data structure. For example, you can look up `:a` in a map:
```clojure
(:a {:a 1 :b 2 :c 3} :a)
; => 1
```
You can provide a default value, as with `get`:
```clojure
(:d {:a 1 :b 2 :c 3} "No gnome knows homes like Noah knows")
; => "No gnome knows homes like Noah knows"
```
Using a keyword as a function is pleasantly succint, and Real Clojurists do it all the time. You should do it too!

### Vectors
A vector is similar to an array, in that it's a 0-indexed collection. For example, here's a vector literal:
```clojure
[3 2 1]
```
Here we're returning the 0th element of a vector:
```clojure
(get [3 2 1] 0)
```
Here's another example of getting by index;
```clojure
(get ["a" {:name "khangtb1"} "c"] 1)
```
You can see that vector elements can be of any type, and you can mix types. Also notice that we're using the same `get` function as we use when looking up values in maps.

You can create vectors with `vector` function:
```clojure
(vector "creepy" "full" "moon")
; => ["creepy" "full" "moon"]
```
You can use the `conj` function to add additional elements to the vector. Elements are added to the `end` of a vector:
```clojure
(conj [1 2 3] 4)
; => [1 2 3 4]
```
Vectors aren't the only way to store sequences; Clojure also has lists.

### Lists
Lists are similar to vectors in that they're linear collections of values. But there are some differentces. For example, you can't retrieve list elements with `get`. to write a list literal, just insert the elements into parentheses and use a single quote at the beginning:

```clojure
`(1 2 3 4)
; => (1 2 3 4)
```
Notice that when the REPL prints out the list, it doesn't include the quote. We'll come back to why that happens later, in Chapter 7. If you want ot retrieve an element from a list, you can use the `nth` function:
```clojure
(nth '(:a :b :c) 0)
; => :a

(nth '(:a :b :c) 2)
; => :c
``` 
I don't cover performance in detail in this book because I don't thinks it's useful to focus on it until after you've became familiar with a language. However, it's good to know that using nth to retrieve an element from a list is slower than using `get` to retrieve from a vector. This is because Clojure has to traverse all n elements of a list to get to the n-th, whereas it only takes a few hops at most to access a vector element by its index.

List values can have many type, and you can create lists with the list function:

```clojure
(list 1 "two" {3 4})
; => (1 "two" {3 4 })
```

Elements are added to the beginning of a list:
```clojure
(conj `(1 2 3) 4)
; => (4 1 2 3)
```

When should you use a list and when should you a vector? A good rule of thumb is that if you need to easily add items to the beginning of a sequence or if you're writing a macro, you should use a list. Otherwise, you should use a vector. As you learn more, you'll get a good feel for when to use which


### Sets
Sets are collections of unique values. Clojure has two kinds of sets: hash sets and sorted sets. I'll focus on hash sets because they're used more often. Here's the literal notation for a hash set:
```clojure
#{"kurt vonnegut" 20 :icicle}
```
You can also use `hash-set` to create a set:
```clojure
(hash-set 1 2 2 2)
; => #{1 2}
```
Note that multiple instances of a value become one unique value in the set, so we're left with a single 1 and a single 2. If you try to add a value to a set that already contains that value (such as `:b` in the following code), it will still have only one of that value:
```clojure
(conj #{:a :b} :b)
; => #{:a :b}
```
You can also create sets from existing vectors and lists by using the set function:
```clojure
(set [3 3 3 4 4])
; => #{3 4}
```
You can check for set membership using the `contains?` function:
```clojure
(contains? #{:a :b} :a)
; => true

(contains? #{:a :b} 3)
; => false

(contains? #{nil} nil)
; => true
```

Here's how you'd use a keyword:
```clojure
(:a #{:a :b})
; => :a
```
And here's how you'd use `get`:
```clojure
(get #{:a :b} :a)
; => :a

(get #{:a nil} nil)
; => nil

(get #{:a :b} "kurt vonnegut")
; => nil
```

Notice that using get to test whether a set contains nil will always return nil, which is confusing. `contains?` may be the better option when you're testing specifically for set membership.

### Simplicity 
you may have noticed that the treatment of data structures so far doesn't include a desciption of how to create new types or classes. The reason is that Clojure's emphasis on simplicity encourages you to reach for the built-in data structure first.
If you come from an object-oriented background, you migh think that this approach is weird and backward. However, what you'll find is that your data does not have to be tightly bundled with a class for it to be useful and intelligible.

