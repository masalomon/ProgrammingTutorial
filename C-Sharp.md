# Language Study: C#

C# is Microsoft's answer to Java.  It is based primarily on both C++ and Java (albeit a later
version of C++ than Java's creators worked off of), and includes and intentionally omits several
features from both, as its designer (principally Anders Heljsberg, at Microsoft) felt was
appropriate.  It was later formalized as an ECMA standard, ECMA-334.

Like Java, C# (/C sharp/) is partially compiled.  That is, C# source code files (suffix `.cs`) is
compiled into bytecode (in the Common Intermediate Language, or CIL), that is run on a virtual
machine environment called the CLR (Common Language Runtime), part of Microsoft's greater .NET
framework.  The .NET framework, CLR, and its CIL bytecode are also the target of other programming
languages such as VB.NET.

## Basic Syntax and Types

As with Java and C++, C#'s syntax is very much based on C.  Whitespace is (mostly) insignificant,
and code blocks and other constructs are delineated by curly braces (`{` and `}`).  The
flow-control statements `if`, `else`, and `switch`, the loop statements `for`, `while`, and `do`,
and the jump statements `break` and `continue` all have the same syntax and functionality, with
some restrictions placed on the `switch` to disallow the bug-prone fall-through.  There is an
additional `foreach` loop construct, to loop over elements in a collection or implementing a
particular interface.  Built-in data types include the integral types `sbyte`, `short`, `int`, and
`long` (8, 16, 32, and 64 bits wide, respectively), their unsigned counterparts `byte`, `ushort`,
`uint`, and `ulong`, and a 16-bit `char` for Unicode characters;  the floating point types `float`
and `double` (32 and 64 bits wide), as well as a `decimal` (128 bit decimal floating point) type;
and the `bool` boolean data type (holding `true` or `false`).  Other supported data types include
the `string` data type, `enum` for enumerated constants, arrays of these (and other) data types,
and `struct` and `class` for user-defined object data types.

### Value Types and Reference Types
C# differentiates between _value types_, and _reference types_.  Value types are somewhat analogous
to Java's primitive types, in that assignment to a value type copies the value, and they are
(normally) passed to functions by value (hence the name).  Two objects of the same value type and
value can be compared for equality bit by bit, but they are never the same object.  This is as
opposed to reference types, objects of which are referred to by a _reference_ which can be passed
between functions or assigned to variables (of different but compatible declared type), yet all
references refer to the same object.  For reference types, a change in the object through one
reference is visible through all the other references, whereas for value types, each variable
refers to a different object.  (An exception is when explicitly passing a variable to a function
by reference, using _ref_, _out_, or _in_.)

User-defined objects are value types if defined by a `struct` declaration, and are reference types
if defined by a `class` declaration.  The keywords for the built-in numeric data types are all
aliases to value types declared as `struct`s in the `System` namespace, as are user-defined `enum`
types.  Objects (`object`), strings (`string`), and arrays are reference types.  For some purposes,
value types are _boxed_ into a corresponding reference type, though this is mostly hidden from the
programmer, except for performance.

## Types in C#

### Unified Type System
C# has a *unified type system* in which all types are derived from other _base types_, which derive
from other base types, and ultimately from the root base type `object` (or rather, the `Object`
class in the `System` namespace, to which `object` is an alias).  (A note on terminology: Being
_derived from_ a _base class_ is analogous to Java's _extending_ a _superclass_.)  Value types
(`struct`, including the built-in numeric types) are (implicitly) derived from the special class
`ValueType` (which, as a class, is itself a reference type), which itself derives from `Object`.
User-defined classes can implicitly derive from `Object`, or explicitly derive from a single other
reference type.  (Multiple inheritance is not allowed, and `struct`s can neither derive from
another type nor be derived from by other types.)  Enumerated types (types defined with the `enum`
keyword) are implicitly derived from the special class `Enum` (which is derived from `ValueType`).

Types - structs and classes - can define functions, called methods, that can be called on objects
that are instances of that type.  When a type derives from a base type, the derived type _inherits_
the functionality of its base type, and base type's methods can be called on object that are
instances of any type that derives from that type.  A derived type can _override_ such a function,
so that when the method is called on instances of that particular derived type (or any types that
subsequently derived types), the derived type's implementation of the function is called instead of
that of the base type.  The determination of which function is called is made at runtime, not at
compile time, and depends not on the declared type of the variable through which the function is
called but on the actual type of the object.  This behavior - a primary characteristic of OOP
(object-oriented programming) is known as _polymorphism_.

An `interface` is a type that declares functions with particular names and signatures (the names
and types of its parameters), but provides no default implementation for derived classes.  A
`class` or `struct` can _implement_ any number of interfaces; the programmer of the derived type
must supply an implementation for each method declared in each of the interfaces it implements
(unless an implementation is supplied by the explicitly named base type of a class).  As for
methods declared in base types, calling a method declared in an interface though a variable
(a) whose declared type is that interface and (b) assigned a reference to an instance of a type
that implements (directly or indirectly, via a base type or sub-interface) that interface will
call the correct implementation of the method based on its actual type at runtime.

The implementation of value types is such that they are instantiated inline, either on the stack or
in whatever data types they are contained.  This means variables of value types cannot be `null`
(i.e. referencing no object) and memory for them need not be allocated.  A constructor can be
called to initialize the object's data.  Reference types, on the other hand, are created on the
heap; the `new` operator allocates storage for them which must later be freed (though this freeing
is done automatically by the CLR's garbage collector).  A constructor is always called explicitly,
though a no-arg constructor may be created automatically if none is supplied by the programmer.  An
assigned variable of reference type that refers to no actual object (instance of the declared type) 
has the value `null`.

### Arrays
An _array_ is a sequence of values in a contiguous block of memory, named by a single variable.  A
one-dimensional array is named by its underlying type and a pair of square brackets, e.g. `int[]`
or `string[]`.  An array is a reference type that (implicitly) derives from the special class
`Array`, which also supplies a number of methods and members that all arrays inherit from it.

Arrays in more than one dimension come in two flavors: true multi-dimensional (_rectangular_)
arrays, allocated as contiguous blocks, and _jagged_ arrays, or arrays containing other arrays.
The syntax for rectangular arrays is a single set of brackets, with the indices for each dimension
separated by commas, as in

    int[,,] threeDimensions = new int[4,4,4];
    threeDimensions[2, 1, 3] = 6;

The syntax for jagged arrays is closer to that of multimensional arrays in Java: A single array
is initialized, its members themselves declared to be references to arrays (of whichever type,
including other arrays), but the member arrays are not allocated; the member references are all
initialized to `null`, and they must be assigned manually afterwards.

Arrays can be created with the `new `_`<type>`_`[`_`<size>`_`]` syntax, which initializes all
members to the type's default value (0, `null`, etc.), or they can be created and assigned by an
array initializer, which sets the array's size(s) (and dimensions) automatically:

    int[] temperatures = new int[10];                       // Array of 10 int's, initialized to 0
    int[] testScores = { 95, 96, 98, 100, 97, 92 };         // Array of 6 int's, initialized.
    // Two-dimensional 4x3 array, stored in row-major order.
    int[,] classSizes = { { 20, 21, 21 }, { 22, 21, 21 }, { 24, 23, 24 }, { 19, 20, 24 } };
    int[][] jagged = {              // Jagged array of integer arrays, each of different size:
       new int[] { 1, 2 },          // Size of 2, initialized; size comes from initializer
       new int[] { },               // Empty int array, size comes from initializer
       new int[] { 3, 4, 5 },       // 3-element array, size comes from intializer
       new int[0],                  // Empty array, size explicitly given
       new int[10],                 // 10-element array, uninitialized(to 0), size explicitly given
       null                         // null reference, to be assigned an int[] at another time.
    } ;

The `Array` class also has a number of `static` methods that can be useful. //TODO

### Strings
Strings are instances of the `string` type (or class `String` in the `System` namespace).  String
instances are immutable; operations which appear to modify strings actually return new `string`
instances with the changes applied.

String literals are delimited by double quotation marks (`"`).  Within a string literal, special
characters and sequences can be escaped by a backslash (`\`).  Special characters are those which
would (or might) otherwise be interpreted differently, such as quotes (which would otherwise close
the string), control characters (BEL, BS, TAB/HT, LF/NL, VT, FF, and CR), and the backslash (`\\`).
Hexadecimal (`\x`_`xxxx`_) and Unicode (`\u`_`uuuu`_) escape sequences can specify any character
not available in the source code's text encoding (e.g. beyond the ASCII character table); Unicode
escape sequences must be exactly four hexadecimal digits long, while hexadecimal escape sequences
are broken by the first non-hex-digit, up to four hex digits.

A string literal can be prefixed by a dollar sign (`$"..."`), which indicates that the string is
_interpolated_.  Within an interpolated string, expressions surrounded by braces are evaluated and
their result inserted into the string, formatted as needed.  (To escape interpolation of an opening
or closing brace, double it: `{{` and `}}`.)  An interpolation expression can be followed by a
comma and a minimum width, and/or by a colon and a format specifier, with an optional precision,
somewhat between C's `printf()` functionality and Java's `MessageFormat` functionality.
(Interpolated strings were introduced in C# 6.0, released in July, 2015.)

_Verbatim_ strings are interpreted verbatim: line breaks can be embedded, and escape sequences are
not interpreted, but instead included verbatim.  (This is especially useful for Windows-style file
pathnames, in which directories are separated by a backslash.)  The exception is a doubled
double-quote (`""`) sequence to include a double-quote in a verbatim string without closing it.
Within a verbatim interpolated string (`$@"..."`), literal braces also need to be doubled; single
braces enclose an expression to be evaluated and interpolated.  (In C# 6.0, interpolated verbatim
strings had to begin with `$@`; as of C# 8.0, the interpolation and verbatim indicators can come in
either order.)

_Raw string literals_ are similar to verbatim string literals, except that indentation is removed
(akin to incidental whitespace in Java's text blocks).  Quotation marks, literal backslashes, and
newlines within a raw string literal don't need to be escaped, and any number of consecutive double
quotes can be included, so long as they are fewer than the number of double quotes delimiting the
string.  A raw string literal is delimited by a 3 or more double quotes, followed by a newline.
A matching number of double quotes, on their own line closes the string.  The newlines immediately
following and preceding the string's delimeters are not included in the content, and whitespace
indentation matching that of the closing delimeter is removed from each line.  A raw string literal
can be interpolated by prefixing the opening quotes with 1 or more `$` - the number of `$`
indicates how many braces surround an interpolated expression.  For example, use two `$$` to
include literal formatted JSON, with `{{...}}` to interpolate expressions.  Or use `$$$` for a
literal formatted mustache or handlebar template, with `{{{...}}}` for interpolation.  (Raw string
literals were introduced with C# 11, release in November, 2022.)

### Enums (Enumerated Types)


### Classes and Structs


### Delegates and Events


## Particulars of C# Syntax
 - `foreach (var` _`<variable>`_ `in` _`<IEnumerable>`_`) ...`
 - `switch` must have block (no single case, as in C/C++.)
 - No implicit fallthrough from `case` to `case`.  Each `case` must end with a jump, either a
    `break`, a `return`, a `throw`, or an explicit `goto case <other>` or `goto default`.
 - A `switch` can also be an expression rather than a statement, in the form
        _`<test-expression>`_ `switch {` _`<pattern>`_ `=>` _`<result>`_`, ... }`
 - A function can indicate that a parameter of a value type is to be passed by reference instead
   of by value:
   - `ref` before a parameter indicates that a parameter is passed by reference.  The call site
    must also include the `ref` before the argument.
    - `out` before a parameter indicates that a parameter will be set in the function.  It is
    passed by reference, but (a) it need not be assigned ("definitely assigned") when being passed
    to the function, and (b) must must be assigned a value by the function.  The call site must
    also prefix the argument with `out`, and the variable can even be declared within the call.
   - `in` before a parameter indicates that a parameter is passed by reference, but it must be
    definitely assigned before the function is called, and the function will not modify it..
   - The return value of a function can also be specified to be by reference, by putting a `ref`
    before the function's return type in its declaration.  The returned value must still be within
    scope when the function returns (i.e. not a local variable).  // TODO
 - A `params` preceding a 1-dimensional array parameter indicates a variadic function, with any
   number of arguments of the specified type of the array (or of compatible type).  The arguments
   are collected into the `params` array, which must be the last parameter in the parameter list.
 - A simple function (one that returns immediately, or evaluates a single expression) can be
   defined in lambda format as  
   _`<function-signature`_ `=>` _`<lambda-type-expression>`_ `;`  
   // TODO something about statement expressions for `void` or `async` functions, same as
   with lambdas.
 - Static methods must be called using the class name, instance methods must be called using
   an instance of a class on which the method is defined.
 - Arguments can be passed to method by name, i.e. the name of the parameter, a colon (`:`), then
   the argument.  Arguments passed this way can appear in any order, and all parameters must be
   present.  This is an addition to the normal way of passing arguments, unnamed and in the same
   order as in the method signature (declaration).  Mixing positional and named arguments is also
   allowed, subject to restrictions that minimize ambiguity.
 - Parameters to a method can be optional, specified by providing a default argument value in the
   method definition.  The default value must be a compile-time constant, like a literal.  It can
   also have the form `default(`_`<type>`_`)` (the default value of a type, i.e. 0, `null`, etc,)
   - or just `default`, as the type can be inferred - or `new `_`<value-type>`_`` (which invokes
   the default constructor for the value type, yielding a similar effect as `default`).  Sensible
   restrictions for avoiding ambiguity apply: optional parameters at the end of a parameter list,
   positional arguments fill parameters in order, and there are compile-time rules for choosing the
   correct overload of a method when more than one may be applicable.

## Particular Rules for C# Classes and Structs
 - An instance method must be declared `virtual` for a derived type to override it.  The derived
   type must explicitly use the `override` keyword to override an inherited method.
 - There is a difference between _fields_ of a class and _properties_ of a class.  Fields are just
   like in other languages: They can be `public`, `private`, or `protected` (or `internal`), they
   belong to instances of an object, and they can be accessed and modified by instance methods of
   the class, derived class, or client code, subject to the access modifier.  Properties are fields
   that have _accessors_, declared in braces after the property name, named `get` and `set`.  These
   become simple `get_`_`<Field>`_`()` and `set_`_`<Field>`_`()` methods, but can have bodies to
   e.g. restrict the domain of the field's values.  Omitting an accessor makes the field read-only
   or write-only; the accessability of an accessor can also be restricted, e.g. `private`.
 - If an accessor is defined with a body are supplied, the storage of the field's value must be
   defined by the programmer explicitly, e.g. with another field.  Otherwise, the _auto property_
   syntax (_`<type> <Property-name>`_ `{ get; set; }`) tells the compiler to take care of storage.
 - The accessor bodies can be _expression-bodied members_ in lambda-style format, with `=>`.  A
   `set` accessor always gets a single parameter named `value`, whether defined with braces or as
   a lambda.
 - A class's property may be an _indexer_ method, which allows an object to be accessed with array
   syntax, like `fibonacci[23]` or `dictionary["key"]`.  The declaration of an index method is
   _`<access> <return-type>`_ `this[`_`<parameter-type> <parameter-name>`_`]`, and the parameter
   type is not restricted to integral types, or to only a single parameter.  Like properties,
   indexers have `get` and/or `set` accessors, and the value being assigned in a `set` accessor is
   given the parameter name `value`.
 - A _iterator method_ can be called a number of times, each successive call picking up where the
   previous call ended.  Each call to the iterator is concluded by a `yield return` statement.
