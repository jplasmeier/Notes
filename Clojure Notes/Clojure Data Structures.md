# Clojure Data Structures

### REPL

* Read - Reader translates code into data structures
* Evaluate - Return a value based on the data structures passed in by the reader
* Print - Print the result 
* Loop

There's emphasis on Read and Evaluate being separate steps. Symbols get evaluated, keywords stand as themselves. Clojure has good data structures. 

Clojure data structures are immuatable. They are also readable by the reader. Since the data is immutable, evaluation is consistent. Comparing two mutable objects by value is pointless because the results are inconsistent. Proper equality means that you can compare by value and know that the result will never change. This is possible through immutable data structures.

Collections are manipulated via interfaces. 
Support sequencing (stepping through)
Support persistent manipulation
Support metadata
Implement Java.lang.iterable
Implement the non-optional (read-only) portion of Java.util.Collection

### Data Structures

#### nil

This means nothing. It has no value. It is a possible value of any type. The same as `null` in Java. `nil` and `false` represent values of logical falsity. 

#### numbers

Pretty much the usual. There are ratio types which preserve the integers in the fraction.

#### strings and characters

Clojure strings are Java strings. Clojure characters are Java Characters. 

#### keywords and symbols

Keywords are symbolic identifiers which map to themselves. Two keywords with the same name are identically the same object. Strings of the same value are not even the same object. 

`(gensym "prefix")` function returns a random symbol with a unique name. Useful for code generation. 

I missed a bit about namespaces.

#### Collection

Collections are immutable. They are also persistent. So what happens when you need to change something? Create a new value which is actually just the reference to the previous value with that one change. You can do this because you know that reference is immutable.  

There is a hierarchy beginning with an interface with 3 functions:

* (count coll) - returns the number of items in the collection
* (conj coll item) - returns a new collection with the item 'added'. Stands for conjoin.
* (seq coll) - returns a new ISeq on the collection. Works on native Java arrays. 


##### conj

`(conj '(a b c) :x) -> '(:x a b c)`

`(conj '[1 2 3] 4) -> '[1 2 3 4]`

Immutable and persistent vectors and hash-tables are a big deal.  

We don't use `conj` on maps too often since keys and values are usually treated independently. But, it is still implemented as adding a map to a map:

`(conj {:a 1, :b 2} {:c 3}) -> {:c 3, :a 1, :b 2}`

Note: Commas are whitespace. 

Sorted maps are maps where the keys are sorted. You need to call a function to create one.

```
(def sm (sorted-map :a 1 :b 2))
(conj sm {:c 3, :d 4}) -> {:a 1, :b 2, :c 3, :d 4}
```

##### seq

Returns a sequence representation of a collection. Lists are identical to their sequence. Other collections' sequences are probably not readable. 

#### lists

Count is O(1). 

* count
* conj
* seq
* peek - return the first item
* pop - return the rest of the list 

`peek` and `pop` are basically just first and rest but the naming is a little more intuitive when using a list as a stack. Lists are stacks from the front. Vectors are stacks from the back. 

`rseq` on a vector is constant time. 

#### vector

A map of indices to values. Effectively constant time access. 

`(assoc [1 2 3] 1 4) -> [4 2 3]`

You can add a value with associate by passing the last index plus one:

`assoc [1 2 3] 3 4) -> [1 2 3 4]`

`get` is the associative function. It returns the value at the index. `nil` is returned if the index is not in the vector. `nth` returns an exception on this. Vectors are fast. 

`peek` and `pop` work on the end. Extremely fast subvector operations: 

`(subvec [1 2 3 4 5] 2 4) -> [3 4])`

The result is a vector. 

#### maps

Many implmenetations of maps. Hashed and sorted. Hashed have hashCode and equals, runs in effectively constant time. Sorted maps use red-black trees and run in `O(log(n))` time. 

`(hash-map keyvals)`
`(sorted-map keyvals)`
`(sorted-map-by comparator keyvals)`

`(assoc map key val)` maps a key to a value, updates if key exists.

`(dissoc map key)` returns the map without the key value pair for that key.

You can pass a third argument to get: a value to return if the key is missing. 

`(contains? m k)` returns a boolean if the key is present.

`(find m k)` returns the map entry for key or `nil` if not present.

`(select m ks)` returns a map containing only those entries whose key is in keys

`(key m-e)` returns the key of a map entry

`(value m-e)` returns the value of a map entry

`(keys m)` returns a sequence of the keys in the map

`(values m)` returns a sequence of the values in the map.

`(merge m+)` merges maps into one map. 

#### intuitive information

Vectors are not evaluated by default, whereas lists are. Only vectors have effectively constant time access/sublist operations. 

Use `rest` instead of `next` when you want the empty list instead of nil to be returned on passing in the empty list/vector.



