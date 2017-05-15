# Clojure: Brave Clojure Examples

## Chapter 3: Do Things

### Refactoring Symmetrizer

Old:

```
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

New:

```
(defn better-symmetrize-body-parts
  "Expects a seq of maps that have a :name and :size"
  [asym-body-parts]
  (reduce (fn [final-body-parts part]
            (into final-body-parts (set [part (matching-part part)])))
          []
          asym-body-parts))
```

Now, instead of mucking about `loop` and `recur` we use `reduce` to apply an anonymous function to the vector of parts. The anonymous function maps and conj's (via `into`) the result from `matching-part` helper into the result.

## Chapter 4: Core Functions

