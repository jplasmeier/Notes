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

### Vampire Analysis Program

Exercises: 

Return a list of names:

```
(defn find-vampire-names
	[records]
	(map #(get % :name) records))
```

Append another suspect:

```
(defn append-suspect                                                                                                    
	[suspect-name suspect-glitter-index suspects]                                             
	(conj suspects {:name suspect-name :glitter-index suspect-glitter-index}))
```

## Chapter 5: Functional Programming

### Peg Game

#### Game Board Data Structure

The data structure used to store the board is important. We will use a map where keys represent the positions on the board and the values represent information about each position. This information will itself be a map from keywords to data. The keywords used are `:pegged` and `:connections` which have booleans and a map as values, respectively. Additionally, the board map will have an entry with key `:rows` and an integer value representing the number of rows in the game. The map might look like this:

```
{1  {:pegged true, :connections {6 3, 4 2}},
 2  {:pegged true, :connections {9 5, 7 4}},
 3  {:pegged true, :connections {10 6, 8 5}},
 4  {:pegged true, :connections {13 8, 11 7, 6 5, 1 2}},
 5  {:pegged true, :connections {14 9, 12 8}},
 6  {:pegged true, :connections {15 10, 13 9, 4 5, 1 3}},
 7  {:pegged true, :connections {9 8, 2 4}},
 8  {:pegged true, :connections {10 9, 3 5}},
 9  {:pegged true, :connections {7 8, 2 5}},
 10 {:pegged true, :connections {8 9, 3 6}},
 11 {:pegged true, :connections {13 12, 4 7}},
 12 {:pegged true, :connections {14 13, 5 8}},
 13 {:pegged true, :connections {15 14, 11 12, 6 9, 4 8}},
 14 {:pegged true, :connections {12 13, 5 9}},
 15 {:pegged true, :connections {13 14, 6 10}},
 :rows 5}
```

In the `:connections` value map, the key represents a legal destination and the value represents the position which is removed. 

#### Functions

The first functions we need to implement deal with creating the board. 