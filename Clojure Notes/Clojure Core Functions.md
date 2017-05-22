# Clojure Core Functions

## Abstract Data Structures

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

## Functions on Sequences

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

#### reduce

`reduce` is a Clojure function that applies a function to a sequence by applying the function to an inital value (if given) or first/second element and applies the result to each successive element. 

```
(reduce + [1 2 3 4])
; => 10
(reduce + 10 [1 2 3 4])
; => 20
```

While we think of `reduce` as taking a list and returning a single element, there's actually no restriction on the return type of `reduce`- it could be a sequence containg more elements than before. An example is filtering a map:

```
(reduce (fn [new-map [key val]]
          (if (> val 4)
            (assoc new-map key val)
            new-map))
        {}
        {:human 4.1
         :critter 3.9})
; => {:human 4.1}
```

One could implement `map` using `reduce`.

#### take, drop

Both `take` and `drop` take a sequence and a number `n`. `take` returns the first `n` elements; `drop` returns the sequence following the first `n` elements:

```
(take 3 [1 2 3 4 5 6 7 8 9 10])
; => (1 2 3)

(drop 3 [1 2 3 4 5 6 7 8 9 10])
; => (4 5 6 7 8 9 10)
``` 

##### take-while, drop-while

The above functions are cases of more general functions: `take-while` and `drop-while`. These functions return values that meet a given predicate function:

```
(def food-journal
  [{:month 1 :day 1 :human 5.3 :critter 2.3}
   {:month 1 :day 2 :human 5.1 :critter 2.0}
   {:month 2 :day 1 :human 4.9 :critter 2.1}
   {:month 2 :day 2 :human 5.0 :critter 2.5}
   {:month 3 :day 1 :human 4.2 :critter 3.3}
   {:month 3 :day 2 :human 4.0 :critter 3.8}
   {:month 4 :day 1 :human 3.7 :critter 3.9}
   {:month 4 :day 2 :human 3.7 :critter 3.6}])

; returns elements until the function returns false
(take-while #(< (:month %) 3) food-journal)
; => ({:month 1 :day 1 :human 5.3 :critter 2.3}
      {:month 1 :day 2 :human 5.1 :critter 2.0}
      {:month 2 :day 1 :human 4.9 :critter 2.1}
      {:month 2 :day 2 :human 5.0 :critter 2.5})

; returns elements until the function returns true
(drop-while #(< (:month %) 3) food-journal)
; => ({:month 3 :day 1 :human 4.2 :critter 3.3}
      {:month 3 :day 2 :human 4.0 :critter 3.8}
      {:month 4 :day 1 :human 3.7 :critter 3.9}
      {:month 4 :day 2 :human 3.7 :critter 3.6})
```

#### filter, some

`filter` returns all elements of a sequence that satisfy a given predicate function. 

```
(filter #(< (:human %) 5) food-journal)
; => ({:month 2 :day 1 :human 4.9 :critter 2.1}
      {:month 3 :day 1 :human 4.2 :critter 3.3}
      {:month 3 :day 2 :human 4.0 :critter 3.8}
      {:month 4 :day 1 :human 3.7 :critter 3.9}
      {:month 4 :day 2 :human 3.7 :critter 3.6})
```

Unlike `take` and `drop`, `filter` will always iterate over all elements in the sequence, which may be undesirable. 

`some` returns the first truthy value matching a given predicate function, or `nil` if none is found. 

```
(some #(> (:critter %) 5) food-journal)
; => nil

(some #(> (:critter %) 3) food-journal)
; => true
```

#### sort, sort-by

`sort` does what you would expect. `sort-by` is a keyed sort which takes in a function as well as the sequence to sort:

```
(sort [3 1 2])
; => (1 2 3)

(sort-by count ["aaa" "c" "bb"])
; => ("c" "bb" "aaa")
```

#### concat

`concat` takes two sequences and returns a new one containing the elements of the first concatenated to the first element of the rest:

```
(concat [1 2] [3 4])
; => (1 2 3 4)
```

### Lazy Seqs

Many of these functions return lazy sequences in the which the values are not evaluated until the values are needed. 

```
(def vampire-database
  {0 {:makes-blood-puns? false, :has-pulse? true  :name "McFishwich"}
   1 {:makes-blood-puns? false, :has-pulse? true  :name "McMackson"}
   2 {:makes-blood-puns? true,  :has-pulse? false :name "Damon Salvatore"}
   3 {:makes-blood-puns? true,  :has-pulse? true  :name "Mickey Mouse"}})

(defn vampire-related-details
  [social-security-number]
  (Thread/sleep 1000)
  (get vampire-database social-security-number))

(defn vampire?
  [record]
  (and (:makes-blood-puns? record)
       (not (:has-pulse? record))
       record))

(defn identify-vampire
  [social-security-numbers]
  (first (filter vampire?
                 (map vampire-related-details social-security-numbers))))
```

This code lazily creates a map of vampire details and social security numbers and filters it on the `vampire?` predicate function. 

#### time

Wrapping functions in a `(time fn)` call will return the elapsed time of running the inner function. In the above example, the function `(time (vampire-related-details 0))` takes just over 1000 msces, because `map` does not actually have to apply `vampire-related-details` to each social security number. 

#### infinite sequences

You can create lazy-sequences of infinite length using `repeatedly`. In this function, `(take 3 (repeatedly (fn [] (rand-int 10))))
; => (1 4 0)`, 3 could be any integer and the sequence returned would be of the same length. 

The `lazy-seq` keyword may be necessary to return a lazy sequence:

```
(defn even-numbers
  ([] (even-numbers 0))
  ([n] (cons n (lazy-seq (even-numbers (+ n 2))))))

(take 10 (even-numbers))
; => (0 2 4 6 8 10 12 14 16 18)
```

## Functions on Collections

If functions on sequences act on the elements of the sequence, then functions on collections act on the collection itself. 

#### into

Into has a number of uses. One of them is that it allows you to return data in a specific data structure (besides seq):

```
(map identity {:sunlight-reaction "Glitter!"})
; => ([:sunlight-reaction "Glitter!"])

(into {} (map identity {:sunlight-reaction "Glitter!"}))
; => {:sunlight-reaction "Glitter!"}

(map identity [:garlic :sesame-oil :fried-eggs])
; => (:garlic :sesame-oil :fried-eggs)

(into [] (map identity [:garlic :sesame-oil :fried-eggs]))
; => [:garlic :sesame-oil :fried-eggs]
```

One can also provide a non-empty argument as an initial value:

```
(into {:favorite-emotion "gloomy"} [[:sunlight-reaction "Glitter!"]])
; => {:favorite-emotion "gloomy" :sunlight-reaction "Glitter!"}

(into ["cherry"] '("pine" "spruce"))
; => ["cherry" "pine" "spruce"]
```

#### conj

`conj` is the Clojure version of lisp's `cons`, Though Clojure has `cons` for lists, `conj` works (differently) on each data structure. On vectors, `conj` adds the (entire) second argument to the end of the vector:

```
(conj [1 2 3] [4])
; => [1 2 3 [4]]

(into [1 2 3] [4])
; => [1 2 3 4]

(conj [1 2 3] "four")
; => [1 2 3 "four"]

(into [1 2 3] "four")
; => [1 2 3 \f \o \u \r]
```

`conj` and `into` are similar and often, functions are defined on rest parameters (`conj`) or on a seq (`into`).

## Function on Functions 

Functions are first-class citizens and can be passed into other functions to create higher-order functions with ease. 

### apply

The `apply` function "explodes" a seq into arguments:

```
(max 1 2 3)
; => 3

(max [1 2 3])
; => [1 2 3]

(apply max [1 2 3])
; => 3
``` 

### partial

The `partial` function takes a function `bar` and any number of arguments `args-bar`, and returns a function `foo`. This function `foo` can take more arguments `args-foo`. Calling `foo` applies the original function `bar` to `args-bar` then applies `bar` to `args-foo`:

```
(def add10 (partial + 10))
(add10 3)
; => 13
(add10 5)
; => 15

(def add-missing-elements
  (partial conj ["air" "water" "earth"]))
  
(add-missing-elements "fire" "ether")
; => ["air" "water" "earth" "fire" "ether"]
```

A more practical example:

```
(defn lousy-logger
	[log-level message]
	(condp = log-level
		:warn (clojure.string/lower-case message)
		:alert (clojure.string/upper-case message)))

(def warn (partial lousy-logger :warn))

(warn "Red light ahead")
; => "red light ahead"
```

### complement

The `complement` function takes a boolean function and returns a function that is the complement of the given function:

```
(defn identify-humans
	[social-security-numbers]
	(filter #(not (vampire?))
		(map vampire-related-details social-security-numbers)))

(def not-vampire? (complement vampire?))
(defn identify-humans
	[social-security-numbers]
	(filer not-vampire?
		(map vampire-related-details social-security-numbers)))
``` 

### comp

The `comp` function composes a function around an inner function. 