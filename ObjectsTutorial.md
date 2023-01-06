# Data Structures

Most computer languages have a way of maintaining large amounts of data in units, known
under various names and with different properties and abilities, the common theme being
that the entire collection can be referred to with a single variable, and shared as a
unit between parts of a program or between a program and a library, the operating system,
or the outside world.

There are primarily a small number of these data structures, some maintained by the
language itself and some maintained by libraries using features of the language.  These
native structures can be expanded upon in a vast number of ways, in large collections of
libraries, and with slight variations can be adapted for any particular need of the
moment.  Each language seems to have its own terminology for the various data structures,
but usually the basic operations on them are pretty similar, and where a certain operation
is not readily available an implementation can be written using the basic operations.

## The Basic Data Structures

### Scalar Variables
A __scalar__ variable is a variable whose data is a single value.  The term scalar is used
mostly to differentiate between such variables and __compound__ variables (those whose
data is more than one single value), and may not even exist in the documentation or
reference manuals of a language, which refer to them simply as variables.  The term
_scalar_ is used explicitly in Perl, in which scalar variable is referenced (or a variable
is referenced in scalar context) by prepending a `$` (dollar sign), that symbol being
reminiscent of an _S_, for scalar.  Other languages (such as Java) may refer to such
variables as _native_, meaning they are directly supported by the underlying VM (virtual
machine) or hardware platform.

A scalar variable can usually hold a single number, whether integers of whichever sizes or
floating point numbers of whichever precision that are directly supported by the language
and/or the underlying hardware platform.  Many languages also recognize an integral type
representing a (printable) character, of either 8 or 16 bits (or both), and a boolean type
representing truth values (_true_ or _false_) that is often a small integer containing the
values 0 or 1.  Some languages extend the concept of scalar to include strings of
characters, while others treat such a _string_ as a compound variable with special support
within the language or core library.

The final sort of data that can be stored in a scalar variable is a _pointer_, or
reference as it is sometimes called.  The value stored in a pointer variable is the
address (in a machine's memory) of another variable (scalar or compound), or some other
way of accessing the value of another variable indirectly.  The idea of a pointer is
that changing its value does not change the value of the underlying variable, it merely
changes which variable is being referenced.  There are many algorithms that operate on
compound objects that rely on being able to reference bits of data without naming them
directly, and the pointer is essential to such algorithms.

### Compound Variables
The simplest kind of compound variables are the _array_ and the _structure_.  In some
languages, a _string_ is also implemented as a programmer-visible compound variable.

### Arrays, Lists, and Sequences
An __array__ is a sequence of variables, each of which does not have its own name but is
accessed as an __element__ of the array, generally indexed by an integer.  On most
(hardware) platforms, an array is implemented as a block of memory allocated as a unit.
The first element of the array is placed at the bottom (or base) of the memory block, and
subsequent elements are placed at successive offsets to the base of the block.  Generally,
the elements of the array are all of the same size (number of bytes), thus the offset
address is the size of the element times the index number of the element.  For this
reason, the index of an array in most languages starts at `0`: the first element is
`array[0]` (sometimes called the "zeroth" element to reflect the index number), the second
element is `array[1]`, and so on.  (The brackets, `'['` and `']'`, are a common way to
indicate the index of element, though some languages use parentheses, `'('` and `')'`.)
Other languages, reflecting the natural (spoken) language, index arrays starting from `1`.
And some languages allow the programmer to choose which indexing method should be used.

The term used for the array can differ among languages and libraries.  Library
implementations of arrays may be based on a language's native array but add features not
supplied by the underlying native array, such as size managment and search capabilities.
Some of the common names are: __vector__ (based on the mathematical concept of a vector),
__list__ (an array is a list of elements), and __sequence__ (based on elements of an array
following each other sequentially, by numbered index).

Some library-based array implementations allow the elements of an array to be of different
types and sizes.  This is generally implemented by the actual elements of the array being
pointers or references to the elements thought of as being stored in the array, though
this may be hidden from the programmer.  This is often a feature of scripting languages or
other weakly typed languages, such as JavaScript, Perl, or most shell scripting languages.

### Structures, Objects, and Maps
A __structure__ is a set of variables that are treated as a unit.  Each variable in the
set, called a __field__ or __member__, is named by a label or tag, and a field and its
corresponding value may be called a __property__ of the structure.

If the structure is implemented natively by the language, the number, type, and order of
the members are often fixed at compile time.  The members' names might not even survive
past the compilation stage, each member's name being reduced to an offset into the block
of memory containing the structure's values.  Such structures are known as __structs__
(from the C keyword used to declare them), or more commonly as as __classes__ or
__objects__ in object-oriented languages.  The most common syntax for a member of a
struct or class is the _dot operator_ (a period, `'.'`), as in _`variable.member`_.

Library implementations of this paradigm, that of elements of an object accessed by keys
(usually strings), are most often called a __map__, as each element in the set of __keys__
is _mapped_ to its corresponding __value__.  Other libraries and languages use names like
__dictionary__ (each term mapped to its definition), __mapping__, __associative array__
(an array indexed by words instead of number), and __hash__ (from the way the keys are
hashed to allow for fast lookup).  Such implementations, included in some scripting
languages like JavaScript, Perl, or the Korn and Bash shells, usually allow adding entries
(key/value pairs) during runtime, as well as operations such getting or iterating over the
set of keys or entries.  The set of keys returned by such an operation may or may not be
in the same order as the keys were added to the map; a set is therefore usually described
as __unordered__, unless specified otherwise.

(Hashing is a way of turning a value, like a string, number, or object, into an integer
that will then be used as an index into an array.  So long as the same key always hashes
to the same integer, that key can be used to store values in a map and get them back
exactly as they were stored.)

### Strings
A __string__ is essentially a sequence of characters, and so is usually stored internally
as an array of character-type integers (__char__ in many languages).  There are two ways
of delineating a string of a certain length: either by including its size as a bit of
metadata, or by appending a string-end character such as an ASCII `NUL` (`'\0'`) at the
end of the string.  Both methods have advantages and disadvantages, but the length marker
is more common than the NUL terminator, especially in languages that include strings as an
(almost) native type and hide the exact implementation from the programmer.

In C/C++, a string is normally a `NUL`-terminated array of characters.  Its allocated size
must be 1 more than its length (the number of characters), and its address is the address
of the first character in the string, degenerating into a __char *__ in most contexts.
The standard C library includes two families of string functions, those that expect C
strings (the _`str***()`_ family) and those will work with strings of a given length (the
_`strn***()`_ family).  C++ also includes a `string` library type (`std::string`) that
does its own memory management and hides the internal string implementation.

Whether a string is a simple (scalar) variable or a compound variable depends on the
language.  In some languages, it straddles the scalar/compound divide, being treated as a
scalar variable in some contexts (e.g. assignment and concatenation) and as a compound
variable (an array or object) in others (e.g. memory management), perhaps with some
dedicated syntax or operators.

## Interfaces and Implementations
The array and structure paradigms are often extended by libraries, sometimes included as
part of the language and sometimes included as part of the language's standard library.
These libraries contain specialized __container__ or __collection__ types, generally
implemented by structures and related functions, or by classes in languages that support
them.  The idea of a collection is that the values stored in a collection need not be
aware (and cannot be aware) that they are being accessed as part of a collection rather
than a simple variable.

An exhaustive list of all the container types and their names is impossible, but here are
some of the well known types, defined by the operations a programmer can perform on them,
their programmer interface:

### List
A _List_ is the logical extension of the array.  It may manage its size, allowing elements
to be added (to the end), inserted (in the middle), or removed (from anywhere) at runtime.
The basic operations are `set(`_`index`_`, `_`value`_`)`, `get(`_`index`_`)`, `size()`, and
`add(`_`value`_`)`; most implementations include `insert(`_`index`_`, `_`value`_`)` and
`remove(`_`index`_`)`, many include some sort of `find()` or `indexOf()` operation.

The defining feature of a List is that of random access, that any element can be accessed,
by its index, at any time, and that adding an element to the List will not change the
index of any element.  Inserting and removing elements at a given index, if implemented,
will change the indices only of those elements with a higher index.

### Stack
A _Stack_ is similar to a List in that is (usually) an extension of an array, but where a
_List_ is random access, a Stack grows and shrinks only from one end, called the _top_ of
the stack, in LIFO (last in, first out) order.  The essential operations on a Stack are
`push(`_`value`_`)` (push a value onto the stack) and `pop()` (remove the value at the top
of the stack and return it), with an additional `peek()` operation (return the element at
the top of the stack without removing it) a common addition.  The term _stack_ is
analogous to a stack of plates or trays at a cafeteria.

A Stack need not be implemented with an underlying array; a common implementation is with
a linked-list (see below) instead.  A Stack may have a limited size; pushing an element
when the Stack is full leads to a _stack overflow error), while popping an element from an
empty Stack leads to a _stack underflow_ error.  The handling of such errors depends on
the library implementation, and is beyond the scope of this document.

### Queue
A _Queue_ (pronounced like 'Q') is similar to a Stack but is analogous to a queue (or
line) of people, where the first one on the line is the first one served.  Elements are
added to the Queue at the back and removed from the front, in FIFO (first in, first out)
order.  The essential operations on a Queue are `add(`_`value`_`)` (also known as
`enqueue()`, adding an element to the back of the queue) and `remove()` (also knows as
`dequeue()`, removing the element at the front of the line).  A common additional
operation is `peek()` (a.k.a. `poll()`, returning the element at the front without
removing it).

As with a Stack, a Queue may not be implemented with an underlying array; a linked-list
or even a doubly linked list is more likely.  And as with a Stack, there may be a may be
a size limitation that will lead to an error when adding an element to a full Queue, and
an underflow error when removing an element from an empty Queue.

Just to round up the discussion, there is a container known as a _Deque_ (pronounced like
"deck"), a double-ended queue that combines the operations of a Stack and a Queue,
allowing elements to be added and removed (or peeked) at both ends.

### Linked List
In an array (or an array-based List), the elements of the array (or the pointers to the
elements) are all (or mostly) in a single block of memory, one after the other.  In a
_Linked List_, the elements are in separate blocks of memory, called __nodes__ (the term
being borrowing from graph theory), with each node containing a pointer (or reference) to
the following node in the list. This makes inserting or removing an element a simple
matter of updating references, as opposed to shifting a block of memory to insert or
remove an element from an array.  Linked lists can be singly linked, with each node
containing only forward references to the next node, or doubly linked, with each node
containing references to the next and the previous nodes, so traversing the list is
possible in either direction.

A Linked List is an implementation of a List, and its operations may be those of a List,
Stack, or Queue.  Its limitation is that it is not random access; the only way to `get()`
the _n_'th element of the list is to traverse all the elements from one end of the list
until the desired index.

### Set
Formally, a _Set_, based on the mathematical concept of a set, is a collection of distinct
elements; whereas a List can contain the same element (or an equivalent element) multiple
times, a Set can only contain it once.  A Set is (by default) unordered, meaning that when
iterating through all the elements of the Set, the elements are not guaranteed to be
served in the same order they were added to the Set.  The essential operations on a Set
are `put(`_`value`_`)` (add an element to the Set, or rather, ensure the Set contains it),
`contains(`_`value`_`)` (check whether a value is an element of the Set), and
`remove(`_`value`_`)` (remove an element from the set).  Many implementations also include
a traversal operation or iterator to get all the elements of the set.

### Map
A _Map_ is the logical extention of the structure.  It consists of a Set of values known
as _keys_, each of which is mapped to a _value_.  A given key can only map to a single
value, though it is possible for multiple keys to map to the same value.  The essential
operations of a Map are similar to those of the Set: `put(`_`key`_`, `_`value`_`)` (map
a key to a value), `get(`_`key`_`)` (return the value the key maps to), and sometimes
`contains(`_`key`_`)` (to check whether there is a mapping for a given key).  Like the
Set, most implementations also include a traversal operation or iterator to get all the
key/value pairs (or entries) in the Map.

In JavaScript, an object is implemented as (sort of) a Map, its field names keys to their
corresponding values.  When the field names are simple strings, the _`object`_`.`_`field`_
notation suffices.  When the field name is ambiguous (a number or words with spaces or
reserved characters) or is being read from a variable, another notation is needed, one
reminiscent of arrays indexing: _`object`_`['`_`field`_`']` or _`object`_`[`_`var`_`]`.  A
similar notation is also used in the Korn and Bash shells for associative arrays (their
term for Maps), `${`_`variable`_`[`_`key`_`]}` or `${`_`variable`_`[$`_`keyVar`_`]}`; Perl
does the same with hashes except for using braces (`{...}`) to indicate the key or index.

### Hash Map and Hash Table
The most common implementation of Maps and Sets is the _Hash Table_ or _Hash map_.  An
array of a certain size (often, but not necessarily, a power of two) contains elements
known as _buckets_.  A key (generally a string) is _hashed_, or transformed into an
integer of a given size by a _hash function_.  This _hash code_, modula the size of the
array, is the index of the bucket where the key and value will be stored.  Ideally, each
key will hash to a different index, but at some point, _hash collisions_ (two keys hashing
to the same index) are unavoidable.  The most common solution is for each bucket to be a
linked list of matching keys and their associated values.  When the hash table grows to a
certain size, the table is enlarged, the keys are rehashed, and the values stored in the
new buckets based their keys' hash codes modula the new size of the table.  The keys being
stored in buckets based on their content rather than the order of insertion is why Maps
generally unordered, unless a separate data structure is used to remember the order of
insertion, such as in Java's LinkedHashMap.  A Set may be implemented by a hash table, the
members as the keys and a single shared value indicating presence.

Essential to the successful use of Maps is a consistent equality test - it must be
possible to check if two keys are "equal", given whichever definition of equality is
desired by the programmer - and a consistent hashing algorithm - hashing the same key
twice must yield the same hashcode.

[Next - Object Hierarchies](ObjectHierarchy.md)  
