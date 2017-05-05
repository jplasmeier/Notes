# Clojure Basics

### Control Flow

Clojure provides `if`, `do`, `when`.

```
; if - you should know what this means
(if boolean
  then
  else)
  
; do - execute sequentially and return the last expression
(do (println "1")
    (println "2")
    (println "3")
    (println "returning...")
    4)
; => 4

; when - conditional do block
(when boolean
	do1
	do2
	do-return)
```

### Booleans/Values

Clojure uses `true` and `false` for boolean truth/falsity and `nil` for nullity. Equality is checked simply using `=`.

`or` returns the first truthy value or the last value. Apparently, strings and keywords are truthy. 

`and` returns the last truthy value or the first falsey value. 

### Binding

`def` binds names to values. You can't change the value of bindings except by naking a new binding to the same name. Either way, syou shouldn't have to, or ever, do this in practice.

### Types

#### Numbers

You've got integers, floats, and ratios.

#### Strings

Use `str` for concatenation. There is no string interpolation.

#### Maps

Clojure version of dictionaries/hashes. Map literals are curly brackets and matching pairs delimited only by white space:

```
{:A "Alpha" :B "Bravo" :C "Charlie"}
```

There's no type enforcement:

```
{:A "Alpha" :B "Bravo" "C" "Charlie" "plus" +}
(({:A "Alpha" :B {:B "Bravo"} "C" "Charlie" "plus" +} "plus") 1 2)
;=> 3
(({:A "Alpha" :B {:B "Bravo"} "C" "Charlie" "plus" +} :B) :B) 
;=> "Bravo"
```

You can also use `hash-map` on key-value pairs to build a map instead of the literal. 

##### get

You can use `get` to access map elements, and also specify what to return (instead of `nil`) if the key is not found:

```
(get {:a "A" :b "B"} :c "not foundo")
;=> "not foundo"
```

##### get-in

You can also do a get operation on nested keys with `get-in`:

```
(get-in {:a {:A "A" :a "a"} :b {:B "B" :b "b"}} [:b :B])
;=> "B"
```
#### Keywords

Keywords are closed identifiers. So, `A` could be bound to something, but `:A` will always mean `:A`. Keywords can also be used as lookup functions:

```
(:A {:A "Alpha" :B {:B "Bravo"} "C" "Charlie" "plus" +})
;=> "Alpha"
```
This supports default failure values just like `get`.

#### Vectors

Effectively just a map from an index to a value. They look like lists but are not under the hood. Log32(n) lookup. 

#### Lists

Standard linked lists, which are fast to append to the head. Slow linear lookup.

#### Sets

Standard set semantics, no duplicates or ordering. Can be constructed with `hash-set` or literals like `#{:A :B :C}`. 