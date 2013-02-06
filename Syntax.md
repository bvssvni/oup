 
#Syntax

The source is first parsed into a syntax tree and then evaluated.  

In Oup, each token alternates between a 'name' and a 'operator'.  

    <name> <op> <name> <op> ...
    
Example:

    var a = 0
    var b = 10
    a = b
    
At first, it looks like most programming languages, except you place arguments  
outside the paranthesis '()':  

    Print () "hello"
    
All operators uses non-alphanumeric characters and all names uses alphanumeric.  
This means the space between the tokens can be ignored.  

    a = 2+1/5

The space within a name or operator can not be ignored.  

All language-specific keywords are put in the beginning of a sentence, such as:

    for ...
    func ...
    if ...
    var ...
    
Nested paranthesis are not allowed, instead you can use sub-expressions:  

    Python:
    
      print(2 * (3 + 5))
    
    Oup:
    
      Print () 2 * .sum {
          var sum = 3 + 5
      }
          
A '.' in front of a name tells "look for this variable among the sub-expressions.  

##Design Mode vs Editor Mode

In Design Mode, one can edit the source like a normal file.  

In Editor Mode, Oup only allow altering of values used with the '=' assigment operator.  
One cannot change expressions that uses the '<-' assigment operator.  
This is used in for-loops to create objects and import from other files:

    for i <- Range () 0, 10 {  
        ...  
    }  
    
    var a <- MyFile.oup  

The source is actually not affecting the run-time, it is an interface where only  
the variables are editable.  
The changes affects the tree directly, without any recompilation.  

##Lists

Lists can be declared by using comma as separator.  
You can mix the types in a list.  

    var a = 1, 2, 3, 4
    var b = "Once", "uppon", "a", "time"
    var c = "I", "am", 67, "years", "old"
    var d = a, b, c
    
Because comma is used for lists, the '.' is used for decimal separator:  

    var a = 1.4, 2.7, 3.323, 4.42309
    
All variables are lists, such that a single number of value is treated a list of length 1.  
To create an empty list, use the keyword 'void':  

    var a = 0     // A list of length 1.  
    var b = void  // A list of length 0.
    
When you pass arguments to a function, you are passing a list.  

    Add () 1, 2    // Argument are lists.
    
To pass multiple lists, use sub-expressions:  

    Join () " ", .sentence {
        var sentence = "hello", "how", "are", "you?"
    }
    
To access a list item by index, use the '[]' operator:  

    var a = 1, 2, 3, 4
    for i <- Index () a
        Print () a [] i

##Comments

Oup uses C++/C# like comment style.  

// for single-line.

/* */ for multi-line.  

##Tags

Oup supports tags as printing statements for easy HTML or XML generation.  
The line must start with the character '<' to be interpreted as a tag.  

    <html>
    
equals
    
    print "<html>"

Example:

    <html>
    <body>
    <h1>Welcome</h1>
    <p>
    Print () .body {
        var body = db.GetContent () void
    }
    </p>
    </body>
    </html>

##Functions

A function call is done by sending arguments after the '()' operator:

    Print () "Hello"
    
The rest of the line is sent as arguments, but you can chain functions:

    var a = A () B () C () void
    
First function 'C' is called with no arguments.  
Then function 'B' is called with the result from 'C'.  
Then function 'A' is called with the result from 'B'.  

Actually, this is not completely accurate, since Oup passes expression nodes.  
It is the responsibility of the function to compute the arguments.  
'A' receives the expression linked to 'B () C () void' and then evaluates it.  
This technique allows a function to be lazy and only compute what it needs.  
Only because there is an expression does not mean it computes anything,  
but it behaves like it would if it computed all the time.  
When and which order to compute expressions is solved by the expression optimizer.  

You can declare your own functions in Oup:

    func sayHello () name {
        Print () "Hello " + name
    }
    
Whatever expression passed to the 'name' argument is passed on to the 'Print' function.  
The expression is not evaluated until it is explicit told to do so by a function.  
In that sense, functions written in Oup are placeholders for calls to the underlying API.  

Operators are just functions as well, there is no 'computation' going on in a function.  
In fact, the dependices are traced back to constants or variables.  

    func add () a, b {
        return a + b
    }
    
    sayHello () "Sven"      // Prints 'Hello Sven'
    var two = add () 1, 1   // two == 2

You can even add it to an object and point to the same function like expressions:

    var a = void {
        func sayHello () name {
            Print () "Hello " + name
        }
    }
    
    var b = void {
        func sayHello = a.sayHello
    }
    
If you want to inherit from another object and change some properties, you can use simple assigment:

    var a = void {
        func doSomething () a, b {
            return a + b
        }
    }
    
    var b = a {
        func doSomething () a, b {
            return a - b
        }
    }
    
You can even replace a function or add a new one to a module:

    a = a {
        func doSomething () a, b {
            return a - b
        }
    }

Because a new function can be declared with same name, there is a strict meaning of which function a name points to.  
Functions are emitted to a manager that keeps control over the scope of a function.  
When referring to a function, the last emitted function with same name before that point is used.  

    func a () void {
        Print () "Hello"  // Call 4
    }
    
    func b () void {
        a () void         // Call 3
    }
    
    func a () void {
        b () void         // Call 2
    }
    
    a () void             // Call 1, prints "hello".  

Notice that functions are not deleted, you can declare a new function with same name but not delete any previous.  

##Ifs

Because the syntax is strictly a tree in Oup, there is no 'elif' or 'else' like in Python.  

    Python:
        if a < 3:
            print("Larger")
        else:
            print("Less or equal")
            
    Oup:
        var cond = a < 3
        if cond == true {
            Print () "Larger"
        }
        if cond == false {
            Print () "Less or equal"
        }

Often it is possible to write it in a simpler way:

    Print () .msg {
        var msg = "Less or equal"
        if a < 3 {
            msg = "Larger"
        }
    }

##'=' Assigment vs '<-' Assigment

Oup has two operators for assignment.  
The first one:

    var a = 0
    
This value tells Oup that the right side can:

1. Be changed if the right side is a single value.  
2. Computed whenever any dependicy changes.  

The other type of assignment computes the expression on the right side just once:

    var a <- 0
    
This means it can not be modified at run-time.  
The '<-' operator requires a space after it, so it's actually '<- '.  
This is to avoid problems with negative numbers.  

##For loop

Oup supports loops to create many objects of similar kind:  

    for i <- Range () 0, 10 {
           var a = block () .x, .y, .w, .h {
              var x = i * 20
              var y = 10
              var w = 5
              var h = 5
           }
    }
    
Even the objects are created in a loop, they are linked to expressions with a static location,  
so you change these properties in the source or in the editor.  
This is possible because the values are emitted to the backend independently of what  
object they belong to.  

##Void

Oup requires an expression to end with a 'name' type.  
The 'void' keyword is used to call a function that takes no parameters.  

    doSomething () void
    
Declaring an unitialized variable is equal to set it to 'void', an empty list:  

    var a = void
    
    var a
    
##Operators

When there is a minus sign '-' after a non-numeric followed by a digit, like

    1+-2
    
or followed by an 'e' at a end of a word that starts with a digit or '-', like

    3e-8
    -3e-8

the minus sign is assumed to belong to a number.  

    =       Assign expression.
    '<- '   Assign value of expression. Notice the required space at end.
    +=      If expression, reuses the existing one and adds another term with + operator.  
            If value, adds the value of the right side to the existing value.  
    -=      ...
    *=      ...
    /=      ...
    %=      ...

    ;       Separation of recursive expression from initial value.
    :       Used for communcation over a socket or channel, left argument is id of object.  
            Example: 3:0,0,255
    
    ,       Separation of list items.
    
    []      Access list by index.
    ()      Call function with following parameters.
    
    ==      Equality check.
    !=      Non-equality check.
    <=      Less or equal check.
    >=      Greater or equal check.
    <       Less check.
    >       Greater check.
    
    \       Except. Raises an error if there are any other operators to the right of it.
    
    +       Add.
    -       Subtract.
    %       Modulus.
            Example: var a = a * 2 ; 1  
    |       Or.
            
    *       Multiply.
    /       Divide.
    &       And.
            
    ..      Creates a range of numbers.
            Example: 1..10
    "       Reads the rest of the line as string, or until it matches another ".  
        \"      Reads " to the current reading string.  
    '       Reads the next character as Char followed by a '.  

    
##Modules
    
If you want to create a module without any data, you can use 'void':

    var MyModule = void {
        ...
    }
    
Any reference to the name 'MyModule' without a '.' after it will make the evaluator throw an error.  

##Public: Large Capital

To make a function, variable or module public, use a large capital in the first letter.  

    var a = 0   // not visible outside this file.
    var A = 0   // visible outside this file.
    
    func a () void {    // not visible outside this file.
        ...
    }
    func A () void {    // visible outside this file.
        ...
    }

##Reserved Keywords

There are only 8 reserved keywords:

    false
    for
    func
    if
    oup     (root object containing standard functions, also used for external files)
    true
    var
    void
    
##Oup Module

The 'oup' keyword is preserved for the root module.  
This is the only module public available that starts with a small capital.  
It contains all the standard functions for Oup.  

Oup modules are named after verbs:

    oup.Convert     // Convert between data types.  
    oup.Calc        // Numbers, PI and stuff.  
    oup.Group       // Combine collections.  
    oup.Sort        // Sort stuff.  

##Casting 
    
Casting of one variable to another is done by using the 'oup' module.  

    oup.Convert.ToNumber () "23"
    oup.Convert.ToString () 23

##Multiple Sources

Oup supports the keyword 'return' at top level to emulate modules.  

    // a.oup
    var a = block () 0, 0, 100, 100
    return a
    
Use the '<-' assigment operator with '.oup' extension to evaluate the file.  
    
    // b.oup
    var b <- a.oup

##Numbers

Like Lua, all numbers in Oup are double.  

The decimal seperator is '.', and 'e' is used for scientific notation:

    23.45
    
    10e7
    
    10e-8
    
    7.2309e3

##Recursive Expressions

You can change the expression attached to a variable:  

    var a = 0
    a = a + 2 * DeltaTime () void

This is not a recursive expression, though.  
The 'a' in the expression refers to the number declared at the line before it.  

This is a recursive expression that provides feedback to the variable:  

    var a = a + 2 * DeltaTime () void ; 0
    
The value is declared last, so it does not "push" other letters when it changes.  
