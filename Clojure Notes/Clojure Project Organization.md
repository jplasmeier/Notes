# Clojure Project Organization

## Namespaces

Namespaces are objects containing maps between symbols/names and variable references. You can be "in" one or more namespaces at a time. 

### def

You can use `def` to add a name and a value to the namespace. 

```
(def great-books ["East of Eden" "The Glass Bead Game"])
; => #'user/great-books

great-books
; => ["East of Eden" "The Glass Bead Game"]
```

This is _interning_ a variable. You can check which names are defined in the current namespace with `(ns-interns *ns*)` and the map of all active? possible? namespaces with `(ns-map *ns*)`.

### deref

Notice that variables names are often printed in *reader form* as `#'user/great-books`. To get the value from a variable in this form do:

```
(deref #'user/great-books)
; => ["East of Eden" "The Glass Bead Game"]
```

### Name Collisions

If you call `def` again and use the same name as in a previous variable interning, you will effectively overwrite that name and only have access to the new contents. 

### Creating and Switching Namespaces

Three functions for creating namespaces:

* `create-ns`
* `in-ns`
* `ns`

#### create-ns

The function `create-ns` takes a symbol and creates a namespace known as that symbol (if there are no other namespaces already using that symbol), and returns the namespace. 

But all this does is create a namespace, which isn't useful by itself.

#### in-ns

The function `in-ns` creates a namespace (given a unique name) and then switches into it. To use names from another namespace, instead of calling the name directly, use a fully qualified symbol:

```
cheese.analysis=> (in-ns 'cheese.taxonomy)
cheese.taxonomy=> (def cheddars ["mild" "medium" "strong" "sharp" "extra sharp"])
cheese.taxonomy=> (in-ns 'cheese.analysis)

cheese.analysis=> cheddars
; => Exception: Unable to resolve symbol: cheddars in this context

cheese.analysis=> cheese.taxonomy/cheddars
; => ["mild" "medium" "strong" "sharp" "extra sharp"]
```

### refer

If you import the names from one namespace into another, you won't need to use the fully qualified symbol before calling variables in that namespace. This is accomplished with `refer`:

```
user=> (in-ns 'cheese.taxonomy)
cheese.taxonomy=> (def cheddars ["mild" "medium" "strong" "sharp" "extra sharp"])
cheese.taxonomy=> (def bries ["Wisconsin" "Somerset" "Brie de Meaux" "Brie de Melun"])
cheese.taxonomy=> (in-ns 'cheese.analysis)
cheese.analysis=> (clojure.core/refer 'cheese.taxonomy)
cheese.analysis=> bries
```

`refer` takes 3 options: `:only`, `:exclude`, and `:rename`.

#### only

Use `:only` and pass in a vector of names to import.

#### exclude

Complement of `:only`.

#### rename

Takes a map where the key is the variable's name in its original namespace and the value is a new name to associate with the value within the new namespace:

```
cheese.analysis=> (clojure.core/refer 'cheese.taxonomy :rename {'bries 'yummy-bries})
cheese.analysis=> bries
; => RuntimeException: Unable to resolve symbol: bries
cheese.analysis=> yummy-bries
; => ["Wisconsin" "Somerset" "Brie de Meaux" "Brie de Melun"]
```

Note that to associate the `clojure.core` namespace you can simply call `(clojure.core/refer-clojure)`.

### Private Functions 

You can make functions private, that is, visible only within the current namespace by appending a dash to its definition (i.e. `(defn-)`).

### alias

One can set an alias, that is, a shorter name for namespaces and use that alias to call the namespace instead of the fully qualified symbol.

## Real Project Organization