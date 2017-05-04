# Python Notes

## The Python Language Reference - Data Model

Core semantics of the language

### Objects, values, and types:

Objects are an abstraction of data.

Objects have:

* Identity: invariant property set on creation, similar to memory address. The `is` operator compares identity. The `id()` function returns the numeric representation of that objects' memory address, representing identity.
* Type: invariant property set on creation. Type determines what operations can be done on an object as well as the set of its possible values. The function `type()` returns the type of an object. 
* Value: Some objects are mutable and can have their values changed after creation. Some objects are immutable and cannot have their values changed after they are set upon object creation. An object's type determines its mutability.

Mutable:

* Dictionaries
* Lists

Immutable:

* Strings
* Numbers
* Tuples

Objects are garbage-collected when they become unreachable.

* Containers: Objects containing references to other objects. Lists, tuples, and dictionaries are all containers. Sometimes a container is immutable but contains a reference to a mutable object. We consider the container to be immuatble even if the the mutable object can change.

Interesting note: `a = 1`, `b = 1` may or may not set references to the same object with value 1. `c = []`, `d = []` refers to two different objects both with the value of an empty list.

### The Type Hierarchy

**None**

None has a single value and there exists one single object with this value. This object is used as the keyword `None` and signifies the absence of a value. None has the truth value False.

**NotImplemented**

NotImplemented has a single value and there exists a single value with this value. This object is used as the keyword `NotImplemented` and is returned when the operation does not support the operands provided. NotImplemented has the truth value True.

**Ellipsis**

Ellipsis has a single value and there exists a single value with this value. This object is used as the keyword `Ellipsis` and is used to indicate `...` syntax within a slice. Ellipsis has the truth value True.

**numbers.Number**

Numeric objects are created by literals and returned as the result of arithmetic functions. Numeric objects are immutable. The meaning of shift and mask operations are meant to be consistent with expectations. There are 3 sub-types:

* **numbers.Integral**
	* **Plain Integers:** represent at least the numbers from -2147483648 through 2147483647 (for 32-bit systems, can be more depending on implementation). Usually if an arithmetic operation returns an integer outside of this range, it is returned as Long type. Sometimes `OverflowError` is thrown. For shift and mask operations, plain integers are represented by 2's complement binary. 
	* **Long Integers:** Integers with range limited only by available virtual memory. For shift and mask operations, a binary representation with a variant of 2's complement is used. Using longs in place of plain integers should have no effect on the result of the operation(s). 
	* **Booleans:** represent the logical values True and False. There are two objects `True` and `False` which are the only Boolean objects. True and False behave like 1 and 0 except in name.


* **numbers.Real**

These numbers are double-precision floating point numbers. The underlying language machine architecture and implementation language determine range and overflow behaviour. 

* **numbers.Complex**

These numbers represent complex numbers via a pair of floats. A complex number `z` uses `z.real` and `z.imag` to access (read-only) the real and imaginary components. 

**Sequences**

Sequences represent finite ordered sets indexed by non-negative numbers. 

The number of elements in a sequence is given by the built-in function `len()`.

Sequences can be sliced. For a sequence `a` and indicies `i, j` we take `a[i:j]` as the elements of `a` with index `k` where `i <= k < j`. 

Extended slicing introduced a "step" such that the expression `a[i:j:k]` returns the elements of `a` with index `x` such that `x = i + n*k` where `n >= 0` and `i <= x < j`.

* **Immutable Sequences**

	* **Strings**: The elements of a string are characters, which are strings of one character. Characters represent (at least) 8 bit bytes and have a non-negative integer associated. The functions `chr()` and `ord()` convert these integers to characters and vice-versa, respectively. 
	* **Unicode**: The elements of a Unicode object are Unicode units (Unicode objects of one item). They are either 16 or 32 bit values (`sys.maxunicode`) representing Unicode ordinals. Use `unichr()` and `ord()` to convert back and forth. The function `unicode()` can be used to convert a string to a Unicode representation.
	* **Tuples**: The elements of a tuple are arbitrary Python objects. Tuples are a comma-separated lists of expressions within parentheses.

* **Mutable Sequences**

	* **Lists**: The elements of lists are arbitrary Python objects. Lists are formed by placing a comma-separated list of expressions within square brackets.

	
### The Type Hierarchy (condensed)

**None**

None has a single value and there exists one single object with this value. This object is used as the keyword `None` and signifies the absence of a value. None has the truth value False.

**NotImplemented**

NotImplemented has a single value and there exists a single value with this value. This object is used as the keyword `NotImplemented` and is returned when the operation does not support the operands provided. NotImplemented has the truth value True.

**Ellipsis**

`Ellipsis` and is used to indicate `...` syntax within a slice. 

**numbers.Number**

Numeric objects are immutable. The meaning of shift and mask operations are meant to be consistent with expectations. 

* **numbers.Integral**
	* **Plain Integers:** represent at least the numbers from -2147483648 through 2147483647.
	* **Long Integers:** Integers with range limited only by available virtual memory. 
	* **Booleans:** `True` and `False`


* **numbers.Real**

These numbers are double-precision floating point numbers.  

* **numbers.Complex**

A complex number `z` uses `z.real` and `z.imag` 

**Sequences**

Sequences represent finite ordered sets indexed by non-negative numbers. 

The number of elements in a sequence is given by the built-in function `len()`.

Sequences can be sliced. For a sequence `a` and indicies `i, j` we take `a[i:j]` as the elements of `a` with index `k` where `i <= k < j`. 

Extended slicing introduced a "step" such that the expression `a[i:j:k]` returns the elements of `a` with index `x` such that `x = i + n*k` where `n >= 0` and `i <= x < j`.

* **Immutable Sequences**

	* **Strings**: 'onecharstringsgohere' 
	* **Unicode**: u'unicodecharactershere'
	* **Tuples**: (obj0, obj1, obj2, ...)

* **Mutable Sequences**

	* **Lists**: [obj0, obj1, ...]
	* **Byte Arrays**: Mutable array of bytes.

**Set Types**

Unordered, finite sets of unique, immutable objects. Supports `len()`.

* **Sets**: A mutable set. Use `set()` constructor to create and methoods like `add()`. 
* **Frozen Sets**: An immutable set. Use `frozenset()` constructor. Hashable.

**Mappings**

Finite sets of objects indexed by arbitrary index sets. Uses subscript notation `a[k]` and supports `len()`.

* **Dictionaries**: A mapping from a set of immutable objects to values. Uses `{}` notation.

**Callable Types**

Types that can be called as functions. Note: argument is given to a specified parameter.

* **User-defined Functions**: An object created by a function definition. Takes a list of arguments. Has the following special attributes:
	* _____doc_____: docstring
	* _____name_____: name
	* _____module____: name of the module the function was defined in
	* _____defaults_____: tuple containing default values of its arguments with default values
	* _____code_____: code object representing the compiled function body
	* _____globals_____: reference to the dict of globals defined in the global namespace of the module in which the function was defined.
	* _____dict_____: namespace supporting arbitrary function attributes
	* _____closure_____: tuple of cells containing bindings for a function's free variables

Arbitrary properties can be set and accessed using dot-notation. 

* **User-defined Methods**: 

Combines a class or class instance with any callable object. Has some special read-only attributes:

	* im_self
	* im_func