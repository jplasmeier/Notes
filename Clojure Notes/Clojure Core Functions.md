# Clojure Core Functions

### Abstract Data Structures

Clojure supports data structures as abstractly as possible. 

#### Sequence

The sequence (seq) collection includes anything that can be processed in sequence. 

Any/all seq(s) must implement:

* first
* rest
* cons

And as a result support:

* map

This includes lists, vectors, maps, and sets:

```
(defn titleize
  [topic]
  (str topic " for the Brave and True"))

(map titleize ["Hamsters" "Ragnarok"])
; => ("Hamsters for the Brave and True" "Ragnarok for the Brave and True")

(map titleize '("Empathy" "Decorating"))
; => ("Empathy for the Brave and True" "Decorating for the Brave and True")

(map titleize #{"Elbows" "Soap Carving"})
; => ("Elbows for the Brave and True" "Soap Carving for the Brave and True")

(map #(titleize (second %)) {:uncomfortable-thing "Winking"})
; => ("Winking for the Brave and True")
```

The reader macro anon func in the map example is needed because casting a map to a seq yields a vector for each key value pair. So, `second` is operating on a vector of a key-value pair and returns the value.

### Indirection/Polymorphism

There are some details that abstraction relies on. For example, `first`, `rest`, and `cons` must be defined differently for different data structures. 

Indirection is a general term for when a single thing has multiple meanings/definitions. 

Polymorphic functions dispatch to different function bodies based on the arguments given.

In terms of seq, Clojure converts lists/maps/vectors/sets to seqs by calling the `seq` function on them:

```
(seq '(1 2 3))
; => (1 2 3)

(seq [1 2 3])
; => (1 2 3)

(seq #{1 2 3})
; => (1 2 3)

(seq {:name "Bill Compton" :occupation "Dead mopey guy"})
; => ([:name "Bill Compton"] [:occupation "Dead mopey guy"])
```

### Functions on Sequences

#### map

Applies a function to elements of a sequence. For n-ary functions, give n collections to `map` and `map` will operate on the elements of the collection in order.

```
; Unary
(map inc [1 2 3])
; => (2 3 4)

; n-ary
(map str ["a" "A"] ["b" "B"] ["c" "C"])
; => 
``` 

This can be useful for combining lists into a list of element-wise collections.

You can also map over functions:

```
(def sum #(reduce + %))
(def avg #(/ (sum %) (count %)))
(defn stats
  [numbers]
  (map #(% numbers) [sum count avg]))

(stats [3 4 10])
; => (17 3 17/3)

(stats [80 1 44 13 6])
; => (144 5 144/5)
```