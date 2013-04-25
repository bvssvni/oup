#List As Basic Type

There are no "single" value types, only lists.

    a : int = 1, 2, 3, 4
    b : double = 1.0, 2.0, 3.0, 4.0
    c : char = "hello"

Lists can not contain other list, instead they are flattened:

    a : int = 1, 2
    b : int = 3, 4
    c : int = a, b  // 1, 2, 3, 4
    
The syntax iterates between operators and tokens.
Functions are declared and passed with arguments after the paranthesis:
    
    add () a, b {
      a ++ b
    }
    
    add () 1, 2
    
When passing arguments to functions, the arguments are not flattened.
Instead the arguments are passed as a 'tree' type.

    a : int = 1, 2, 3
    b : tree = a, a // ( 1, 2, 3 ), ( 1, 2, 3 )

This is a class:

    Point () x : double, y : double {
      X = x
      Y = y
    }

Since there is no single objects, you can pass lists to classes:

    x = 1, 2, 3
    y = 2, 3, 4
    c = Point () a, b
    
There is a group object that acts like a set for indices:

    a : group = 1:10
    b : group = 8:12
    c : group = 20:30
    d : group = c + a - b // 1:8, 20:30
    
The ':' operator is only necessary first time you declare a variable.
If you want the compiler to detect the type for you, write:

    a := 1, 2, 3, 4
   
List types:

    int
    double
    char
    
All types that begin with a small letter is reserved as keywords.

The '[]' operator is used to access items from a list:

    a := 1, 2, 3, 4
    b := a [] 0:2   // 1, 2
    
It is not allowed with nested expressions.
Instead, use sub-expressions:

    b := 1, 2, 3, 4
    a := 1:.n {
      n := 2
    }
    
Members beginning with a small letter accessed outside of class
are reserved keywords.

    length    Returns the length of list.
    size      Retuns the size of group.
              Only available on group types.
    sum       Returns the sum type of list.
              Only available on numeric lists as int or double.
    product   Returns the product of items on list.
              Only available on numeric lists.
    mean      Returns the average value.
    median    Returns the median value.
    
    a := 1, 2, 3, 4
    b := a.sum   // 10
    
You can print out to standard output by not assigning to a variable:

    1, 2, 3
    
Objects are treated as tables of columns,
where operations are performed per column,
for example when using '.sum':

    x := 1, 2, 3, 4
    y := 2, 3, 4, 5
    a := Point () x, y
    a.sum   // Point { X = 10, Y = 14 }
    
