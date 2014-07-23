schema
======

**This is a WIP design document**

A schema for defining serialization formats.

The schema consists of 7 fixed-size types, 3 variable-sized types and two kinds of user defined types:

Schema     | Description
------     | -----------
`bool`     | a boolean, either false or true
`int8`     | 8bit signed twos complement integer
`int16`    | 16bit signed twos complement integer
`int32`    | 32bit signed twos complement integer
`int64`    | 64bit signed twos complement integer
`float32`  | single precision floating point number
`float64`  | double precision floating point number
`string`   | utf8 variable-length string
`option<T>`| zero or one element of type `T` (aka **nullable**)
`array<T>` | zero or more elements of type `T` (aka **list**)
*struct*   | a user-defined record type, possibly generic (aka **product type**)
*enum*     | a user-defined tagged union type, possibly generic (aka **sum type**)

While we only *need* the last entry in this table, the rest of the types are added to make it easier to understand how the types map to the various programming languages and serialization formats.

The syntax for the schema is concise but C-like. Here's how you could define a person:

```c
struct Person {
    Age : int32
    Name : string
    Email : option<string>
}
```

A team might contain a list of members:

```c
struct Team {
    Name : string
    Members : array<Person>
}
```

The alignment of a widget may be one of three values, like an enum:

```c
enum Alignment {
    Top
    Middle
    Bottom
}
```
    
The syntax tree for a small language may need values attached to each tag:

```c
enum Expression {
    Constant(Value : int32)
    Add(X : Expression, Y : Expression)
    Multiply(X : Expression, Y : Expression)
}
```

A binary tree might as well be generic:

```c
enum Tree<T> {
    Leaf(Value : T)
    Branch(Left : Tree<T>, Right : Tree<T>)
}
```
    
The compiler enforces that built-in types and keywords are lowercase, while user-defined types start with an upper-case letter.

There is a binary format and a JSON format.

```c
{
    "schema": "http://example.com/schema/1.1",
    "type": "Tree<Team>",
    "value": ["Leaf", {
        "Name": "Mojos",
        "Members": [
            {
                "Age": 21,
                "Name": "Johanna"
            }
        ]
    }]
}
```
    
