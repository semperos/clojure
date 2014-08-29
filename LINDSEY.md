# Lindsey: Where are the parentheses? #

Lindsey is a revised syntax for Clojure. It retains all the power of Clojure's Lisp features, but with a (perhaps) less threatening appearance.

```
function hello []
  println("Hello, world!")
end
```

Local variable bindings:

```
function hello [name]
  let [greeting = "Hello",
       sentence = str(greeting, name, "!")]
    println(sentence)
  end
end
```

## Is this still Clojure? ##

Lindsey is an extension to Clojure's reader/parser, **nothing more.** All of Lindsey's changes to Clojure have been made as a separate `LindseyReader` that produces the same output that Clojure's default `LispReader` does, but that allows for a Ruby-like syntax instead of a fully parenthesized Lisp syntax. The examples above translate as follows:

```
function hello []
  println("Hello, world!")
end
```

Becomes:

```clj
(defn hello []
  (println "Hello, world!"))
```

And the more involved:

```
function hello [name]
  let [greeting = "Hello",
       sentence = str(greeting, name, "!")]
    println(sentence)
  end
end
```

Becomes:

```clj
(defn hello [name]
  (let [greeting (str "Hello " name)]
    (println greeting)))
```

Any changes made to the `RT` and `Compiler` classes have been to allow the `LindseyReader` to be used instead of the `LispReader` by setting the `*current-reader*` dynamic var.

## Progress ##

Currently the Lindsey reader can:

 * Transform `function ... end` forms into `(defn ...)` forms
 * Transform `let [a = 1, b =2] ... end` forms into `(let [a 1 b 2] ...)` forms
 * Function invocation as `println(a, b, c)`
 * Conditional `if...elseif...else...end` implemented

Todo:

 * Infix "environment" for easier math, e.g. `infix a + b * c / d end` (TBD whether or not an order of operations will be introduced)
 * Reserved form transformations:
     * `macro ... end` &raquo; `defmacro`
     * `protocol ... end` &raquo; `defprotocol`
     * `record ... end` &raquo; `defrecord`
     * Conditionals (TBD how cond, case, etc. will work)
     * multi-methods
 * Redesigned `gen-class` and `gen-interface`, thoughts:
     * `class <name> <opts> ... end` &raquo; `gen-class`
     * `interface ... end` &raquo; `gen-interface`
     * `method <name> <opts> ... end` for method definitions
