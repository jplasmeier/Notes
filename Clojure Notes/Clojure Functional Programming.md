# Clojure: Functional Programming 

### Pure Functions 

Pure functions are functions such that:

* referential transparency: the same input gets the same output every time the function is called.
* no side effects: nothing outside of the function is changed 

Examples of functions which are not pure are `println`, `rand`, `slurp`, etc. 

### Control Flow

Instead of looping over an array, use `map` or `reduce` or recursion. 

### Writing Functional Code

In Clojure, it is common to use parameter overloading to distinguish between the base case and the recursive case:

```
(defn sum
    ([vals] (sum vals 0)) 
  ([vals accumulating-total]
       (if (empty? vals)  
       accumulating-total
       (sum (rest vals) (+ (first vals) accumulating-total)))))
```

This is the accumulator passing style. Notice the tail call in the recursive step. There's an `if` statement to check for nullity.

#### recur

However, it's more performant to use `recur`:

```
(defn sum
  ([vals]
     (sum vals 0))
  ([vals accumulating-total]
     (if (empty? vals)
       accumulating-total
       (recur (rest vals) (+ (first vals) accumulating-total)))))
```

### comp

`comp` takes a number of functions and returns a single function that composes the given functions:

```
((comp inc *) 2 3)
; => 7
```

A more practical example:

```
(def character
  {:name "Smooches McCutes"
   :attributes {:intelligence 10
                :strength 4
                :dexterity 5}})
(def c-int (comp :intelligence :attributes))
(def c-str (comp :strength :attributes))
(def c-dex (comp :dexterity :attributes))

(c-int character)
; => 10

(c-str character)
; => 4

(c-dex character)
; => 5
```

### memoize

You can wrap recursive functions in a call to `memoize` and have the results of calls cached:

```
(defn sleepy-identity
  "Returns the given value after 1 second"
  [x]
  (Thread/sleep 1000)
  x)
(sleepy-identity "Mr. Fantastico")
; => "Mr. Fantastico" after 1 second

(sleepy-identity "Mr. Fantastico")
; => "Mr. Fantastico" after 1 second

(def memo-sleepy-identity (memoize sleepy-identity))
(memo-sleepy-identity "Mr. Fantastico")
; => "Mr. Fantastico" after 1 second

(memo-sleepy-identity "Mr. Fantastico")
; => "Mr. Fantastico" immediately
```

