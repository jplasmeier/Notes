# Clojure Functions Reference

### Defining a Function

```
(defn						; keyword
function-name				; what to call it
docstring  				; optional
[list of parameters]	; input parameters as a list
(body of "function"))	; the actual function body
```
### Anonymous Functions

Functions need not have a name. Anonymous functions comprise 3 parts:

```
(fn 				; keyword
[param-list]		; input parameters as a list
function body)	; the actual function body
```

### Concepts

* arity overloading
* destructuring
* & (rest parameter)

### Commonly Used Core Functions

* map
* reduce
* into (map + conj)

### List of Functions

#### Control Flow

* let
* loop
* recur
* if
* while
* do

#### Sequences

* map
* reduce
* take
* take-while
* drop
* drop-while
* filter
* some
* sort
* sort-by

#### Collections

* into