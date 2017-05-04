# Clojure Functions

### Basics

Five parts of a function declaration:

```
(defn						; keyword
function-name				; what to call it
docstring  				; optional
[list of parameters]	; input parameters as a list
(body of "function"))	; the actual function body
```

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

