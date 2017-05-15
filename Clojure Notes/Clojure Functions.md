# Clojure Functions

Clojure functions are first-class citizens, that is, they can be passed like data into other functions (higher-order functions). 

### Basics

Five parts of a function declaration:

```
(defn						; keyword
function-name				; what to call it
docstring  				; optional
[list of parameters]	; input parameters as a list
(body of "function"))	; the actual function body
```

### Special Forms & Macro Calls

Lowkey these are things that look like functions but are not. That is, one can expressions with these terms, but they are not functions. `If` is an example of a special form:

```
(if (condition)
  (then-form)
  (else-form))
```

because calling `if` does not evaluate all of its arguments as a function would. You can't pass special forms as parameters. 


### Arity Overloading

```
(defn multi-arity
  ;; 3-arity arguments and body
  ([first-arg second-arg third-arg]
     (do-things first-arg second-arg third-arg))
  ;; 2-arity arguments and body
  ([first-arg second-arg]
     (do-things first-arg second-arg))
  ;; 1-arity arguments and body
  ([first-arg]
     (do-things first-arg)))
```

This is a useful way to implement optional parameters with default values:

```
(defn x-chop
  "Describe the kind of chop you're inflicting on someone"
  ([name chop-type]
     (str "I " chop-type " chop " name "! Take that!"))
  ([name]
     (x-chop name "karate")))
```

This is the same as say `def x-chop(name, chop_type=None):` in Python.

You could also do something completely different for a different arity, but why would you?

### Rest Parameter

When you include extra parameters to a function, put the extras in a list with a given name. 

```
(defn codger-communication
  [whippersnapper]
  (str "Get off my lawn, " whippersnapper "!!!"))

(defn codger
➊   [& whippersnappers]
  (map codger-communication whippersnappers))

(codger "Billy" "Anne-Marie" "The Incredible Bulk")
; => ("Get off my lawn, Billy!!!"
      "Get off my lawn, Anne-Marie!!!"
      "Get off my lawn, The Incredible Bulk!!!")
```

### Destructuring

You can index elements of a list parameter via destructuring. 

```
(defn chooser
  [[first-choice second-choice & unimportant-choices]]
  (println (str "Your first choice is: " first-choice))
  (println (str "Your second choice is: " second-choice))
  (println (str "We're ignoring the rest of your choices. "
                "Here they are in case you need to cry over them: "
                (clojure.string/join ", " unimportant-choices))))

(chooser ["Marmalade", "Handsome Jack", "Pigpen", "Aquaman"])
; => Your first choice is: Marmalade
; => Your second choice is: Handsome Jack
; => We're ignoring the rest of your choices. Here they are in case \
     you need to cry over them: Pigpen, Aquaman
```

You can also do this with maps, and get named parameters.

```
(defn announce-treasure-location
➊   [{lat :lat lng :lng}]
  (println (str "Treasure lat: " lat))
  (println (str "Treasure lng: " lng)))

(announce-treasure-location {:lat 28.22 :lng 81.33})
; => Treasure lat: 100
; => Treasure lng: 50
```

There's a couple different syntaxes for this:

```
(defn announce-treasure-location
  [{:keys [lat lng]}]
  (println (str "Treasure lat: " lat))
  (println (str "Treasure lng: " lng)))
```

You can also retain reference to the original map input:

```
(defn receive-treasure-location
  [{:keys [lat lng] :as treasure-location}]
  (println (str "Treasure lat: " lat))
  (println (str "Treasure lng: " lng))

  ;; One would assume that this would put in new coordinates for your ship
  (steer-ship! treasure-location))
```

### Function Body

The body of the function is a list of expressions to evaluate. The read of the last expression is returned:

```
(defn illustrative-function
  []
  (+ 1 304)
  30
  "joe")

(illustrative-function)
; => "joe"
```

### Anonymous Functions

Functions need not have a name. Anonymous functions comprise 3 parts:

```
(fn 				; keyword
[param-list]		; input parameters as a list
function body)	; the actual function body
```
Here are some examples:

```
(map (fn [name] (str "Hi, " name))
     ["Darth Vader" "Mr. Magoo"])
; => ("Hi, Darth Vader" "Hi, Mr. Magoo")

((fn [x] (* x 3)) 8)
; => 24
```

This is like a `lambda` in Scheme. 

It comes with this ultra compact syntax using `#` and `%`.

```
(#(* % 3) 8)
; => 24
```
With map:

```
(map #(str "Hi, " %)
     ["Darth Vader" "Mr. Magoo"])
; => ("Hi, Darth Vader" "Hi, Mr. Magoo")
```

These are actually something called _reader macros_. They also support multiple arguments:

```
(#(str %1 " and " %2) "cornbread" "butter beans")
; => "cornbread and butter beans"
```
As well as rest parameters:

```
(#(identity %&) 1 "blarg" :yip)
; => (1 "blarg" :yip) 
```

### Closures in Clojure

Functions can return functions, which are closures and include all the variables that were in scope when the closure was created:

```
(defn inc-maker
  "Create a custom incrementor"
  [inc-by]
  #(+ % inc-by))

(def inc3 (inc-maker 3))

(inc3 7)
; => 10
```

### Pulling It All Together

```
(defn matching-part
  [part]
  {:name (clojure.string/replace (:name part) #"^left-" "right-")
   :size (:size part)})

(defn symmetrize-body-parts
  "Expects a seq of maps that have a :name and :size"
  [asym-body-parts]
  (loop [remaining-asym-parts asym-body-parts
         final-body-parts []]
    (if (empty? remaining-asym-parts)
      final-body-parts
      (let [[part & remaining] remaining-asym-parts]
        (recur remaining
               (into final-body-parts
                     (set [part (matching-part part)])))))))
```

There's a lot going on here.

#### let

The `let` expression binds values to identifiers, just like it does in Scheme. The wrinkle here is that the rest parameter `&` is used in the `let` form. In this case, `part` is bound to the first value in `remaining-asym-parts` and `remaining` is bound to the rest of the values in `remiaining-asym-parts`. 

#### into

The `into` function is a combination of `map` and `conj`. More specifically, in this case, `set` is called to create a Clojure set out of the vector containing `part` and the value resulting from `(matching-part part)`. This is done to ensure that no duplicates are added into `final-body-parts` because Clojure sets do not allow duplicates, and `matching-part` may return a duplicate. 

I suppose this is a fairly popular idiom- one can construct a vector without fear of duplicates by using `(into [] (set (foo)))` where `(foo)` is some form that may return duplicate values.

#### loop

The `loop` form allows for iteration. It can act as a target for `recur`. The bindings in the `loop` expressions are names bound to an initial value. Then, code in the body, often with `recur`, can operate on these values. 

```
; A loop that sums the numbers 10 + 9 + 8 + ...

; Set initial values count (cnt) from 10 and down
(loop [sum 0 cnt 10]
    ; If count reaches 0 then exit the loop and return sum
    (if (= cnt 0)
    sum
    ; Otherwise add count to sum, decrease count and 
    ; use recur to feed the new values back into the loop
    (recur (+ cnt sum) (dec cnt))))
```
