Introduction
============

Laser-D is a computer programming language based on the D programming Language. The language differs from D in some major ways: Laser-D does not support 
garbage collection and does not provide a runtime. As a consequence, Laser-D does not support certain features of the D programming language. Laser-D 
does not support the standard D library phobos for the same reason.

Here is a hello world program in Laser-D.

::

    import core.stdc.stdio : printf;

    extern (C) void main()
    {
        printf("Hello World!\n");
    }


Notation
========
The syntax is specified using Extended Backus-Naur Form (EBNF)::

    Production  = production_name "=" [ Expression ] "." .
    Expression  = Alternative { "|" Alternative } .
    Alternative = Term { Term } .
    Term        = production_name | token [ "…" token ] | Group | Option | Repetition .
    Group       = "(" Expression ")" .
    Option      = "[" Expression "]" .
    Repetition  = "{" Expression "}" .

Productions are expressions constructed from terms and the following operators, in increasing precedence::

    |   alternation
    ()  grouping
    []  option (0 or 1 times)
    {}  repetition (0 to n times)

Lower-case production names are used to identify lexical tokens. Non-terminals are in CamelCase. Lexical tokens are enclosed in
double quotes ``""``.

The form ``a … b`` represents the set of characters from ``a`` through ``b`` as alternatives. The horizontal
ellipsis ``…`` is also used elsewhere in the spec to informally denote various enumerations or code snippets that are not further specified. 


Basic Concepts
==============

Laser-D programs are structured as modules that can be compiled separately and linked with external libraries to create 
native libraries or executables.

Each Laser-D source file defines a single module. 

Each laser-D module contains one or more declarations of entities. 

An entity is a named variable, function, structure or union type, enumeration type, template, mixin template or an external symbol.

Entities in a Laser-D module can have linkage that specifies how these entities may be accessed from other Laser-D modules, or from programs in other 
other languages such as C or C++.

Source Code Representation
==========================

Source code must be Unicode text encoded in UTF-8.

Characters
----------

The following terms are used to denote specific Unicode character classes:

``newline`` 
    the Unicode code point U+000A

``unicode_char``
    an arbitrary Unicode code point except ``newline``

``universal-character-name``
    as defined in ISO/IEC 9899:1999(E) Appendix D of the C99 Standard

::

    nondigit = "A" … "Z" | "a" … "z" | "_" | universal-character-name .
    digit    = "0" … "9" .


Lexical Elements
================

Keywords
--------

The language has following keywords.

::

    abstract    alias        align          asm          assert      auto
    body        bool         break          byte         case        cast
    catch       cdouble      cent           cfloat       char        class
    const       continue     creal          dchar        debug       default
    delegate    delete       deprecated     do           double
    else        enum         export         extern       false       final
    finally     float        for            foreach      foreach_reverse    function
    goto        idouble      if             ifloat       immutable    import
    in          inout        int            interface    invariant    ireal
    is          lazy         long           macro        mixin        module
    new         nothrow      null           out          override     package
    pragma      private      protected      public       pure 
    real        ref          return         scope        shared       short
    static      struct       super          switch       synchronized
    template    this         throw          true         try          typeid
    typeof      ubyte        ucent          uint         ulong        union
    unittest    ushort       version        void         wchar        while
    with        __FILE__     __FILE_FULL_PATH__          __MODULE__   __LINE__
    __FUNCTION__             __PRETTY_FUNCTION__         __gshared    __traits     
    __vector    __parameters


Operators and Punctuation
-------------------------

::

    /     /=    .    ..    ...   &
    &=    &&    |    |=    ||    -
    -=    --    +    +=    ++    <
    <=    <<    <<=  >     >=    >>=
    >>>=  >>    >>>  !     !=    (
    )     [     ]    {     }     ?
    ,     ;     :    $     =     ==
    *     *=    %    %=    ^     ^=
    ^^    ^^=   ~    ~=    @    =>
    #


Identifiers
-----------

::

    identifier = nondigit { nondigit | digit } .


An identifier is a sequence chracters with a ``nondigit`` character followed by ``nondigit`` or ``digit`` characters.  

Lowercase and uppercase letters are distinct. There is no specific limit on the maximum length of an identifier.

Comments
--------

Comments serve as program documentation. There are two forms:

* Line comments start with the character sequence // and stop at the end of the line.
* General comments start with the character sequence /\* and stop with the first subsequent character sequence \*/.

String Literals
---------------

A string literal is either a double quoted string, a wysiwyg quoted string, a delimited string, a token string, or a hex string.

Names
=====
In a Laser-D program, identifiers can be used to denote:

* modules
* structures and union types
* template types and template parameters
* functions and function arguments
* aliases
* labels
* variables
* mixin templates
* import bindings
* attributes
* external symbols

The same identifier can denote different entities at different points in the program.

Scope
-----
For each different entity that an identifier designates, the identifier is visible (i.e., can be used) only within a region of program text 
called its scope. There are following kinds of scopes: global, module, struct or union type, template type, function, block. 

Different entities designated by the same identifier either have different scopes, or the identifiers must be in an overload set.


Modules
=======

Each Laser-D source file represents a distinct module. Each module has a name, optionally qualified with a package prefix.
The module name, if not declared explicitly using the ``module`` declaration, is derived from the source file name.  

If present, the ``module`` declaration must be the first and only such declaration in the source file, 
and may be preceded only by comments and #line directives.

::

    ModuleDeclaration             = "module" ModuleFullyQualifiedName ";" .
    ModuleFullyQualifiedName      = ModuleName | Packages "." ModuleName .
    ModuleName                    = identifier .
    Packages                      = PackageName | Packages "." PackageName .
    PackageName                   = identifier .

The fully qualified module name forms part of the qualified name of every entity declared in that module. Thus two entities with identical names in different
modules can be disambiguated using the fully qualified names of the entities.




