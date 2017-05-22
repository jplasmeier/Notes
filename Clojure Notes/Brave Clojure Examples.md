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

#### Board Creation Functions

The first functions we need to implement deal with creating the board. I didn't take many notes while working through them, but I will give a quick review (working backwards):

##### new-board

```
(defn new-board
  "Creates a new board with the given number of rows"
  [rows]
  (let [initial-board {:rows rows}
        max-pos (row-tri rows)]
    (reduce (fn [board pos] (add-pos board max-pos pos))
            initial-board
            (range 1 (inc max-pos)))))
```

This is the `new-board` function which, given a number of rows, returns a new game board. The `let` bindings are: 

* `initial-board` which is a map with only the entry for the number of rows in the board. 
* `max-pos` which is the maximum position for the given number of rows

The function then reduces over each position from 1 to `max-pos`, starting with `initial-board` and applying an anonymous function that calls `add-pos` on the board. Without knowing exactly what/how `add-pos` does/works, we can see that each invocation per position adds that position to the board, starting with the initial board. 

##### add-pos

```
(defn add-pos
  "Pegs the position and performs connections"
  [board max-pos pos]
  (let [pegged-board (assoc-in board [pos :pegged] true)]
    (reduce (fn [new-board connection-creation-fn]
              (connection-creation-fn new-board max-pos pos))
            pegged-board
            [connect-right connect-down-left connect-down-right])))

(add-pos {} 15 1)
{1 {:connections {6 3, 4 2}, :pegged true}
 4 {:connections {1 2}}
 6 {:connections {1 3}}}
```

The `add-pos` function takes a board, maximum position, and position. It returns a new board with the position added, as well as its possible connections. The `let` binding is:

* `pegged-board` which returns a new board with a new key for `pos` and the nested key `:pegged` with the value `true`

The function then reduces over a vector of functions. Each function is responsible for adding a connection in one of the three directions. Note that there are in fact at most six directions for a peg to move, but those functions are symmetric by design so only three are needed. That is, `connect-right` suffices to implement `connect-left` and so forth. Each invocation calls an anonymous function that applies the `connect-*` function to the board, starting  with the `pegged-board` binding.

Without knowing how the `connect-*` functions work, we can see that this returns a board with each of the possible connections added. 

##### connect-*

```
(defn connect-right
  [board max-pos pos]
  (let [neighbor (inc pos)
        destination (inc neighbor)]
    (if-not (or (triangular? neighbor) (triangular? pos))
      (connect board max-pos pos neighbor destination)
      board)))

(defn connect-down-left
  [board max-pos pos]
  (let [row (row-num pos)
        neighbor (+ row pos)
        destination (+ 1 row neighbor)]
    (connect board max-pos pos neighbor destination)))

(defn connect-down-right
  [board max-pos pos]
  (let [row (row-num pos)
        neighbor (+ 1 row pos)
        destination (+ 2 row neighbor)]
    (connect board max-pos pos neighbor destination)))
```

These functions are responsible for creating new connections (when possible). Each takes a board, maximum position, and a position for which to add connections to. Calculating the value of the target position is accomplished with some triangle arithmetic. Then, these values are passed into a general `connect` function.

##### connect

```
(defn connect
  "Form a mutual connection between two positions"
  [board max-pos pos neighbor destination]
  (if (<= destination max-pos)
    (reduce (fn [new-board [p1 p2]]
              (assoc-in new-board [p1 :connections p2] neighbor))
            board
            [[pos destination] [destination pos]])
    board))
```

The `connect` function takes a board, maximum position, a position, its neighbor, and its destination and returns a new board with the new connection for that position. 

First, we check to see if the destination value is less than or equal to the maximum position value. Certainly it would be invalid to add a position greater than the maximum for the initially-given number of rows. Next, the function reduces on a vector of two vectors, where each inner vector contains a position/destination pair. Each invocation of the anonymous function associates with a key of the position to the nested key `:connections` to the nested key of the destination the value of the neighbor. The neighbor is the peg to be removed when the peg is moved from the position to the destination. 

##### Sanity Check

Let's make sure we're not adding any impossible/invalid connections. By definition every position on the board has a down-left and down-right except those in the last row. The check for that is to simply make sure that any connection is going to a position less than or equal to the maximum position. We perform this check in the general `connect` function. 

Since we call the `connect-*` functions on valid positions, we need not worry about accidentally adding up-left or up-right connections, since these are added in their respective down functions starting from valid positions. 

Then, for `connect-right`, we just need to make sure that we aren't going past the board. This is accomplished in its function by checking that neither the neighbor nor the position is a triangular number. The position value being triangular means that it is the "last" position in a row. Therefore, when a neighbor is triangular, it means that the destination of a rightward connection will actually be on the next row which is erroneous. When a position is triangular, its neighbor will be on the next row (along with its destination) which is also erroneous. 

##### Board Functions Concluded

There are a few more helper functions defined before `connect`, but they are straightforward utility functions: 

* `row-num` which takes a position and returns its row
* `row-tri` which takes a row number and returns its last position
* `triangular?` which takes a position and returns true if it is triangular
* `tri` which takes a number `n` such that `n` triangular numbers are returned

#### Moving Pegs

These functions deal with moving pegs in a valid manner. Most of these are quick one-liners, but there are a couple functions regarding valid moves that are more complicated. Again, we will start with the last function and work backwards. 

##### can-move?

```
(defn can-move?
  "Do any of the pegged positions have valid moves?"
  [board]
  (some (comp not-empty (partial valid-moves board))
        (map first (filter #(get (second %) :pegged) board))))
```

This predicate function returns true if there are still moves left to be made, else false when the game is over. 

Remember that `some` returns the first truthy value for a given predicate function and sequence. So in this case, we return the first value in `(map first (filter #(get (second %) :pegged) board))` that satisfies `(comp not-empty (partial valid-moves board))`. 

Which values comprise that sequence? Since `filter` acts on sequences, the map `board` is converted to a sequence of (nested) vectors. The anonymous function `#(get (second %) :pegged)` takes an entry of this sequence (a vector of two elements, the key and its value) in, calls `second` to extract the value of the position, and calls `get` with `:pegged` to provide truthy/falsity for the `filter` predicate. Any position in the board that satisfies this predicate (has a peg in it) is returned by the filter function. So we take a sequence of all of the pegged positions and then use a predicate to check if the result of calling `valid-moves` with each position will return at least one truthy value. 

##### valid-moves

```
(defn valid-moves
  "Return a map of all valid moves for pos, where the key is the
  destination and the value is the jumped position"
  [board pos]
  (into {}
        (filter (fn [[destination jumped]]
                  (and (not (pegged? board destination))
                       (pegged? board jumped)))
                (get-in board [pos :connections]))))
```

This function takes a board and position and returns the valid moves for that position, or `nil` if there aren't any. The resulting valid moves are in the form of a map where the key is the destination and the value is the jumped position/neighbor. 

This is accomplished by filtering into a map. The filter predicate takes a vector of two elements and checks that the destination is empty and that the neighbor is pegged. This is applied to the value of the (nested) key `:connections` in the value of the key `pos`. 

So, for a position, if there are connections such that the neighbor is pegged and the destination is empty, then that connection yields a valid move. 

#### Rendering the Board

