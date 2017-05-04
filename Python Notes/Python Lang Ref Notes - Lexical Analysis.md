# Python Notes

## The Python Language Reference - Lexical Analysis	
Core semantics of the language

### Lexical Notation: 

~~~~
name      ::=  lc_letter (lc_letter | "_")*
lc_letter ::=  "a"..."z"
~~~~

So, a name comprises lowercase letters and underscores.

Python uses 7-bit ASCII characters.

### Definitions

* Parser: Evaluates the semantics of the token stream.
* Lexical Analyzer: Generates tokens based on the structure of the source file.
* Token: A symbol that means something

### Line Structure

Logical lines end at a NEWLINE token.

Physical lines end at an EOL sequence (CR/LF).

Question: Is LF == NEWLINE in Unix?

Comments (#) are *not* tokens.

To declare the encoding of a source file, use this on the first or second line of the source file:

~~~~
# -*- coding: <encoding-name> -*-
~~~~

### Line Joining

**Explicit Line Joining**

When you need a longer logical line to have more than one physical line, use a backslash:

~~~~
if 1900 < year < 2100 and 1 <= month <= 12 \
   and 1 <= day <= 31 and 0 <= hour < 24 \
   and 0 <= minute < 60 and 0 <= second < 60:   # Looks like a valid date
        return 1
~~~~
*(code copied from the docs)*

**Implicit Line Joining**

However, backslashes do not continue tokens, except string literals. So, for anything using parentheses, square brackets or curly braces, use implicit line joining:

~~~~
month_names = ['Januari', 'Februari', 'Maart',      # These are the
               'April',   'Mei',      'Juni',       # Dutch names
               'Juli',    'Augustus', 'September',  # for the months
               'Oktober', 'November', 'December']   # of the year
~~~~
*(code copied from the docs)*

### White Space

**Blank Lines**

Logical lines containing spaces, tabs, form feeds, or comments are ignored and no newline token is generated. 

An entirely blank line terminates a multi-line statement. 

**Indentation**

Leading whitespace is used to compute identation and determines the grouping of statements. 

The amount of whitespace is pushed onto a stack. Examining the trichotomy of indentation number being read vs. the indentation number at the top of the stack: 

* Less than: There is less whitespace than the number at the top. The number being read must exist somewhere in the stack. A DEDENT token is generated for each number in the stack and each number greater than or equal to this one is popped off the stack.
* Equal to: The current whitespace is the same as the value at the top of the stack. Continue.
* Greater than: The current whitespace is more than the value at the top of the stack. INDENT token is generated and the number is pushed to the top of the stack. 

Whitespace between tokens is used to distinguish between tokens if concatenating the tokens results in different semantic behaviour. 

## Other Tokens

* Identifiers
* Keywords
* Literals
* Operators
* Delimiters

Whitespace characters are _not_ tokens! 

### Identifiers

~~~~
identifier ::=  (letter|"_") (letter | digit | "_")*
letter     ::=  lowercase | uppercase
lowercase  ::=  "a"..."z"
uppercase  ::=  "A"..."Z"
digit      ::=  "0"..."9"
~~~~

So, identifiers comprise lower and upper case letters a through z and arabic numerals. Case is significant. Length is unlimited.

**Reserved classes of identifiers**

* `_*`: Not imported from `from module import *`.
* `__*__`: System-defined names. Defined by the implementation.
* `__*`: Class private names.

### Keywords

~~~~
and       del       from      not       while
as        elif      global    or        with
assert    else      if        pass      yield
break     except    import    print		None*
class     exec      in        raise
continue  finally   is        return
def       for       lambda    try
~~~~

**None is a constant, not a formal keyword, but you cannot assign an object to it.*

Reserved words of the language. Cannot be used as names of objects.

### Literals

Literals are notation for constant values of certain built-in types.

**String Literals**

~~~~
stringliteral   ::=  [stringprefix](shortstring | longstring)
stringprefix    ::=  "r" | "u" | "ur" | "R" | "U" | "UR" | "Ur" | "uR"
                     | "b" | "B" | "br" | "Br" | "bR" | "BR"
shortstring     ::=  "'" shortstringitem* "'" | '"' shortstringitem* '"'
longstring      ::=  "'''" longstringitem* "'''"
                     | '"""' longstringitem* '"""'
shortstringitem ::=  shortstringchar | escapeseq
longstringitem  ::=  longstringchar | escapeseq
shortstringchar ::=  <any source character except "\" or newline or the quote>
longstringchar  ::=  <any source character except "\">
escapeseq       ::=  "\" <any ASCII character>
~~~~

Use matching single or double quotes. 

Some strings can be prefixied with `u` for unicode strings or with `r` for raw strings. 

Escape with a backslash.

String literals separated by whitespace are treated as one concatenated string. 

**Numeric Literals**

Python has 

* Plain integers
* Long integers
* Floating Point numbers
* Imaginary numbers

as types of numeric literals. The `-` in `-1` is an expression combining the unary operator `-` and the numerical literal `1`.

**Integer and long literals**

~~~~
longinteger    ::=  integer ("l" | "L")
integer        ::=  decimalinteger | octinteger | hexinteger | bininteger
decimalinteger ::=  nonzerodigit digit* | "0"
octinteger     ::=  "0" ("o" | "O") octdigit+ | "0" octdigit+
hexinteger     ::=  "0" ("x" | "X") hexdigit+
bininteger     ::=  "0" ("b" | "B") bindigit+
nonzerodigit   ::=  "1"..."9"
octdigit       ::=  "0"..."7"
bindigit       ::=  "0" | "1"
hexdigit       ::=  digit | "a"..."f" | "A"..."F"
~~~~

So, we denote long integers with `L`, and oct/hex/bin as `0o`, `0x`, `0b`, as usual. Plain integers are limited to 2147483647 for 32-bit systems. Long integers are only limited by available memory.

**Floating Point literals**

~~~~
floatnumber   ::=  pointfloat | exponentfloat
pointfloat    ::=  [intpart] fraction | intpart "."
exponentfloat ::=  (intpart | pointfloat) exponent
intpart       ::=  digit+
fraction      ::=  "." digit+
exponent      ::=  ("e" | "E") ["+" | "-"] digit+
~~~~

So, floating point numbers use a decimal point `.` or exponential notation `e`.

**Imaginary literals**

~~~~
imagnumber ::=  (floatnumber | intpart) ("j" | "J")
~~~~

So, we represent imaginary numbers with the letter j at the end. These are just imaginary numbers, so to form complex numbers you must include a non-zero real part by the addition operator.

### Operators

Python has these operators:

~~~~
+       -       *       **      /       //      %
<<      >>      &       |       ^       ~
<       >       <=      >=      ==      !=      <>*
~~~~

**`<>` is not the preferred nomenclature, dude. Use `!=` instead, which means the same thing.*

### Delimiters

~~~~
(       )       [       ]       {       }      @
,       :       .       `       =       ;
+=      -=      *=      /=      //=     %=
&=      |=      ^=      >>=     <<=     **=
~~~~

Of these delimeters, the following subset have specuial meaning or are used in other tokens:

~~~~
'       "       #       \
~~~~

The following ASCII characters are not used (except in string literals):

~~~~
$       ?
~~~~









