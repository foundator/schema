schema
======

A schema for defining serialization formats.

The schema consists of 6 fixed-size types, 3 variable-sized types and two kinds of user defined types:

Schema   | Description
------   | -----------
`int8`   | 8bit signed twos complement integer
`int16`  | 16bit signed twos complement integer
`int32`  | 32bit signed twos complement integer
`int64`  | 64bit signed twos complement integer
`float32`| single precision floating point number
`float64`| double precision floating point number
`string` | utf8 variable-length string
`T?`     | zero or one element of type `T` (aka **nullable** or **optional**)
`T*`     | zero or more elements of type `T` (aka **array** or **list**)
*struct* | a user-defined record type, possibly generic (aka **product type**)
*union*  | a user-defined tagged union type, possibly generic (aka **sum type**)

The syntax for the schema is concise but C-like. Here's how you could define a person:

    struct Person {
        Age : int32
        Name : string
        Email : string
    }
    
A team might contain a list of members:

    struct Team {
        Name : string
        Members : Person*
    }
    
The alignment of a widget may be one of three values:

    union Alignment {
        Top
        Middle
        Bottom
    }
    
The syntax tree for a small language may need values attached to each tag:

    union Expression {
        Constant(Value : int32)
        Add(X : Expression, Y : Expression)
        Multiply(X : Expression, Y : Expression)
    }

A binary tree might as well be generic:

    union Tree<T> {
        Leaf(Value : T)
        Branch(Left : Tree<T>, Right : Tree<T>)
    }
    
The compiler enforces that built-in types and keywords are lowercase, while user-defined types start with an upper-case letter.
