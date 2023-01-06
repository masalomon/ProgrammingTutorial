# Data Structures - Part 2

## Trees and Hierarchies
Given the previously described data structures, large amounts of data can be stored in
hierarchical models known as __trees__.  A List can contain a number of elements, each of
which is a scalar, a Map, or another List.  A Map contains mappings from scalars (or other
objects) to scalars, Lists, or other Maps.  This recursion can go on indefinitely, Maps
containing Lists containing Maps and so on.

### Tree
There are dedicated _Tree_ data structures, where each _node_ can contains links (pointers
or references) to other nodes in addition to data.  Generally, a **leaf** node is a node
that contains only data, a **branch** node (which may or may not contain its own data)
contains links to one or more **child** nodes (which may or may not contain links back to
their **parent** node), and two children of the same parent are said to be **siblings**.
A parent node with no parents of its own is the **root** node of a tree, and the **depth**
of a node is the number of links between it and the root node.  A Tree can be thought of
as a generalization of a linked list, which can be thought of as a tree with nodes limited
to one child each.

Different types of trees are named either by their use or by constraints placed upon the
way nodes are added.  For example, a _binary tree_ is a tree where no node has more than
two children, and a _balanced tree_ is a tree where child nodes are added evenly to all
branches of each node, leading to each node at a given depth having the same number of
_descendants_.  A _sorted tree_ is a tree in which a node's children are ordered based on
some sorting algorithm, and a _sorted binary tree_ is one way to sort data as it is read
from some source of input.

### Object Hierarchies in JSON
**JSON** (JavaScript Object Notation) is a notational language (i.e. a data format rather
than a programming language) that describes structured data.  It contains the three types
described above:
- Primitive types (scalars): strings, numbers, booleans, and `null`
- Arrays (lists): ordered sequences of zero or more values
- Objects (maps): unordered sets of name/value pairs (the name must be a string)

The values - the elements of arrays and properties of objects - can in turn themselves
be primitives, arrays, or objects, in the simple data hierarchy described above.

JSON, borrowing syntax from JavaScript, uses brackets (`[...]`) to delineate an array and
braces (`{...}`) to delineate an object.  Within an array, elements are separated by a
comma (`','`); within an object, a name is separated from its object with a colon (`':'`)
and elements (name/value pairs) are separated from each other with a comma.  Strings are
delimited with double quotes (`"..."`), and contain Unicode characters and C-like escape
sequences (`\b`, `\f`, `\t`, `\n`, `\r`, `\"`, `\\`, `\/`, and `\u`_`XXXX`_ for any
Unicode character).  The literals are `true` and `false` for the boolean type, `null`
for the null type, and decimal numbers in either floating point or decimal notation.


### Object Hierarchies in YAML
**YAML** (YAML Ain't Markup Language) is a notational language that is designed to require
less boilerplate (fewer notational symbols) than JSON, making it more human-readable.  It
contains the three general types described above (scalars, lists, and maps), but allows
them to be denoted in either _flow_ or _block_ style.  The flow style for _collections_
(sequences and mappings) and the quoting rules for strings are similar enough to JSON that
valid JSON content is also valid YAML content.  The block style uses line breaks and
indentation (like Python) to indicate nesting and the _scope_ (extent) of sequences and
mappings.

- A _scalar_ is a string of uninterpreted (Unicode) text.  The application using YAML is
  free to interpret the text as numbers, booleans, timestamps, or a string, but YAML's
  rules only specify which text is part of the value.
  - In flow style, a string scalar may or may not be quoted.  Unquoted (_plain style_)
    scalars are the easiest to read, but the content is sensitive to context _indicators_
    such as the comma (denoting the next element in a sequence or mapping), the colon
    (separating a mapping key from its value), or the hash symbol (denoting a comment),
    and leading and trailing whitespace will be trimmed.

    Double quoted scalars are surrounded by double quote characters (`"..."`), within
    which C-style escape sequences are recognized (as in JSON).  Lines are folded, with
    leading and trailing whitespace on each line trimmed unless it is escaped.  To
    preserve a line break, add an extra line containing only whitespace; multiple empty
    lines are all folded into a single newline.

    Single quoted scalars are surrounded by single quote characters (`'...'`), within
    which no escape sequences are recognized except for the repeated single quote (`''`),
    used to denote a single quote character that should not end the string.  Lines are
    folded, with leading and trailing whitespace on each line trimmed, it and the line
    break reduced to a single space.  A line break is preserved by doubling it, i.e. an
    extra new line.  With either quoting style, indicators within the string are not
    recognized.

  - In block style, a string scalar is denoted by a block scalar _style indicator_, an
    optional _header_ containing a _chomp_ indicator and/or an indentation indicator, with
    the content of the block, beginning on the next line, indented (with only spaces) one
    level more than its parent.
    - The block style indicator controls whether the block is taken to be _literal style_,
      all line breaks and leading and trailing whitespace preserved (the `'|'` indicator),
      or _folded style_, line breaks and its adjacent leading and trailine whitespace
      folded into a single space (the `'>'` indicator).  In the folded style, a line that
      is more indented than the block is not folded; its leading spaces (minus the
      indentation level spaces) and surrounding line breaks are preserved.
    - The chomp indicator controls how the final line break and any trailing empty lines
      are processed, either by stripping (all being removed, `'-'` indicator), clipping
      (line break preserved, trailing lines removed; default, no indicator), or keeping
      (preserving both the final line break and trailing empty lines, `'+'` indicator).
    - An indentation indicator is an integer, controlling how many spaces the content is
      indented by, and therefore how many spaces should be removed from each line when the
      content is processed.  If no indicator is given, this defaults to the indentation of
      the first (non-empty) line.  This indicator allows the first line to include leading
      space in its content, for example.

- A list, or **sequences**, is an ordered collection of entries (or values), each of which
  can be a scalar, another sequence, or a mapping.
  - In flow style, sequences are delineated by brackets (`[...]`) and separated from each
    other by a comma (`','`), as in JSON.
  - In block style, sequences are delineated by a leading dash (`'-'`) (followed by a
    space) before each entry in the sequence, which are separated from each other by a
    line break.  All _nodes_ (values) in the same sequence must have the same number of
    spaces (not tabs) preceding the `'-'` _indicator_.
- A map, or **mapping**, is an unordered collection of key/value pairs, where each value
  can be a scalar, a sequence, or another mapping.  Each key in a mapping must be unique,
  and scalars, sequences, and mappings are all valid as keys (with limits).
  - In flow style, sequences are delineated by braces (`{...}`), with a colon (`':'`)
    separating a key from a value and a comma (`','`) between pairs.  To avoid ambiguity,
    a space is generally required after the `':'`, unless the key and value are quoted,
    i.e. JSON-style.
  - In block style, the value is separated from its key by a colon (`':'`) and a space,
    with each key/value pair on its own line, and with the same indent (number of spaces,
    no tabs).  A value can also begin on the next line, indented further than the key.
  - A _complex key_ is when the key in a mappping is not a flow-style scalar.  A complex
    key begins with a question mark (`'?'`) indicator (and a space), and ends at the
    value indicator (an unescaped colon, `':'`).
- Flow style and block style can be mixed, with a flow style mapping containing a block
  style scalar, for example.
- A comment begins with a hash sign (`'#'`) and continues until the next line break.  It
  must be preceded by whitespace, to avoid ambiguity, unless it's the first character on
  a line.
- YAML also allows nodes to be repeated to avoid duplication.  The first occurence of the
  node is identified by an _anchor_ indicator, an ampersand (`'&'`), followed by a word,
  preceding the node (scalar, sequence, or mapping).  Subsequent occurences are referenced
  by an _alias_ indicator, an asterisk (`'*'`), followed by the same same word.  An anchor
  can be redefined; aliases will reference the most recent definition.

## Element Trees
There are some data formats that extend the simple list and map paradigm to that of
_elements_ and _attributes_.  In this paradigm, the object hierarchy is a _tree_ of
elements.  Each element has a name, a set of attributes, and a number of _children_.
Perhaps the most widely used data format in this category is HTML, followed by other
markup languages, many of them applications of XML or SGML.  An element tree can and often
will be implemented using lists and maps, but that detail is hidden by the higher level
conceptualization.  This data structure can be illustrated with a description of XML.

### XML Syntax
XML (EXtensible Markup Language) is a markup language format used for a number of data
formats.  It is a restricted subset of the older SGML, which has a much wider syntax and
looser parsing rules. An XML _document_ contains (besides the XML declaration and a
_document type declaration_) a single _root element_, the root node of the element tree.
An element begins with its _start **tag**_, the element's _tag name_ surrounded by angle
brackets (`'<...>'`).  A start tag can contain a set of _attributes_ between the tag name
and the closing angle bracket.  Each attribute consists of the attribute name, an equals
sign (`'='`), and the attribute value, enclosed in quotes (single or double, but must
match).  The element's _content_, whether plain text or child elements, follows the start
tag.  The element ends with its _end tag_, a slash (`'/'`) preceding the tag name,
urrounded by angle brackets (`</...>`).  An end tag cannot have attributes.  A special
_self-closing tag_ denotes an element with no content.  It consists of a slash immediately
preceding a tag's closing angle bracket (`<.../>`).

The special characters meaningful to the XML parser (e.g. `'<'` and `'>'`) cannot be
escaped; instead, such characters must be replaced with the corresponding _entity_.  An
entity consists of an ampersand, the entity's name, and a semicolon.  For example, a
literal ampersand is represented by the entity `&amp;`.  Comments begin with `<!--` and
end with `-->`.  Comments do not nest, and they cannot appear inside a tag.  Markup and
tags are not recognized inside comments, but they cannot contain a literal `--`.  Special
_CDATA sections_ escape blocks of text containing characters that would otherwise be
recognized as markup.  These sections begin with `<![CDATA[` and end with `]]>`; there is
no way to escape the CDATA end string within a CDATA section.

### XML Document Tree
The root of an XML document tree is its root element.  An element can have a number of
children, whether child elements or child text nodes.  It can also have a number of
attributes, each a key/value pair.  The set of attributes can be implemented as a map,
and the children can be implemented as a list.  What makes such a tree different from a
regular list/map hierarchy is that each element also has a _name_, sometimes referred to
as a _type_ in other contexts.  This relationship can be simply expressed in XML:

    <root-element parent-attribute="pa-value">
      <child-element child-attribute="ca-value">
        This is the text content of the child element.
      </child>
    </root>

But in JSON or YAML, this relationship is more complicated, as a given element has,
separately, a set (or list) of attributes and a list of children, and the element's name
or type is part of the elements representation, as opposed to just a way to access the
elements content.  Perhaps, with some extra boilerplate:

    {
      tag: "root-element",
      attributes: {
        "parent-attribute": "pa-value"
      },
      children: [
        {
          tag: "child-element"
          attributes: {
            "child-attribute": "ca-value"
          },
          children: [
            "This is the text content of the child element"
          ]
        }
      ]
    }

A YAML document could be a little shorter and simpler, something like:

    /root-element:
      parent-attribute: pa-value
      /child-element[0]:
        child-attribute: ca-value
        text-node[0]:
          This is the text content of the child element

Instead, most data available as a document node can be accessed through some sort of DOM
(Document Object Model), where each element has the operations of both a set, to access
its attributes, and a list, to access its children.  Some DOMs (like the W3C DOM for HTML)
consider an element two lists, one of child nodes (including text nodes) and one of child
elements (non-element children are excluded).

[Prev - Objects and Data Structures](ObjectsTutorial.md)  
[Next - JCR: The Java Content Repository](JcrTutorial.md)  
