oup
===

Oup - A content generation programming language (under work).

##The Purpose of Oup

1. Optimize expressions.  
2. Emit objects to backend.  
3. Display editable values of objects in real-time.  
4. Allow the user to edit the values for rapid prototyping and debugging.  

##What Is Oup?  

Oup is a syntax strict programming language for content generation.  
It is designed to be used in animation and games content creation.  
Everything is organized like a tree, where the 'private' variables can be written as sub-expressions.  
It allows one to hide the details of one object without creating conflicts or needing separate files.  

For example, we have a simple game scene with 2 blocks:  

    Block () 0, 200, 100, 10  
    Block () 110, 200, 100, 10  
    
To make it clearer what the numbers mean with the numbers, we can write it like this:  

    var x1 = 0
    var y1 = 200
    var w1 = 100
    var h1 = 10
    Block () x1, y1, w1, h1
    
    var x2 = 110
    var y2 = 200
    var w2 = 100
    var h2 = 10
    Block () x2, y2, w2, h2

This is messy and easy to make mistakes, but in Oup we can write this way:  

    Block () .x, .y, .w, .h {
        var x = 0
        var y = 200
        var w = 100
        var h = 10
    }
    Block () .x, .y, .w, .h {
        var x = 110
        var y = 200
        var w = 100
        var h = 10
    }
        
Now it is easier to create more blocks by duplicating the code we have.  
Since we have 3 values shared between the two blocks, we can write like this:  

    var a = Block () .x, .y, .w, .h {
        var x = 0
        var y = 200
        var w = 100
        var h = 10
    }
    var b = Block () .x, a.y, a.w, a.h {
        var x = 110
    }

Or simply:

    var a = Block () .x, .y, .w, .h {
        var x = 0
        var y = 200
        var w = 100
        var h = 10
    }
    var b = a {
        var x = 110
    }

All variables that are assigned to a number, text or boolean can be changed dynamically,  
and even make the source code update in real-time.  
You can move one of the blocks in an editor and the blocks will move together.  
If we want the blocks to move up and down with time:

    var a = Block () .x, .y, .w, .h {
        var x = 0
        var y = 200 + .wave {
            var wave = .amplitude * Sin () .phaseExpr {
                var amplitude = 20
                var phase = 0.3
                var phaseExpr = 2 * PI * phase * DeltaTime () void
            }
        }
        var w = 100
        var h = 10
    }
    var b = Block () .x, a.y, a.w, a.h {
        var x = 110
    }

The parser reads the code above as following:  

    NAME                TYPE            DEPENDICES
    start               Expr            -
    a                   Block           a.x, a.y, a.w, a.h
    a.x                 Number          -
    a.y                 Expr            a.y.wave
    a.y.wave            Expr            a.y.wave.amplitude, a.y.wave.phaseExpr
    a.y.wave.amplitude  Number          -
    a.y.wave.phase      Number          -
    a.y.wave.phaseExpr  Expr            a.y.wave.phase
    a.w                 Number          -
    a.h                 Number          -
    b                   Block           b.x, a.y, a.w, a.h
    b.x                 Number          -

An expression tells how a variable shall behave, when it should be updated and what it depends on.  
Oup is designed to be interactive, so it exposes the syntax tree to other editors.  
An editor can change the source code without concerning about the exact location of the variable.  

For example, when we write:

    var a = 10
    
This value can be changed and make updates to all runtime objects that depend on it.  
However, the editor can keep a copy of the original source so you can go back if you do not like the changes.  

Each statement can be traced back to an origin in the source.  
The syntax tree in Oup detects whether something is editable or not.  
You can build an editor that modifies the source code, without knowing the exact location of each setting.  

##Syntax

The source is first parsed into a syntax tree and then evaluated.  

In Oup, each token alternates between a 'name' and a 'operator'.  

    <name> <op> <name> <op> ...
    
Example:

    var a = 0
    var b = 10
    a = b
    
At first, it looks like most programming languages, except you place arguments  
outside the paranthesis '()':  

    print () "hello"
    
All operators uses non-alphanumeric characters and all names uses alphanumeric.  
This means the space between the tokens can be ignored.  

    a = 2+1/5

All language-specific keywords are put in the beginning of a sentence, such as:

    for ...
    func ...
    if ...
    var ...
    
Nested paranthesis are not allowed, instead you can use sub-expressions:  

    Python:
    
      print(2 * (3 + 5))
    
    Oup:
    
      print () 2 * .sum {
          var sum = 3 + 5
      }
          
A '.' in front of a name tells "look for this variable among the sub-expressions.  

##Design Mode vs Editor Mode

In Design Mode, one can edit the source like a normal file.  

In Editor Mode, Oup only allow altering of values used with the '=' assigment operator.  
One cannot change expressions that uses the '<-' assigment operator.  
This is used in for-loops to create objects and import from other files:

    for i <- range () 0, 10 {  
        ...  
    }  
    
    var a <- MyFile.oup  

The source is actually not affecting the run-time, it is an interface where only  
the variables are editable.  
The changes affects the tree directly, without any recompilation.  

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
    print () .body
        var body = db.GetContent () void
    </p>
    </body>
    </html>

##Functions

You can declare your own functions in Oup:

    func sayHello () name {
        print () "Hello " + name
    }
    
    func add () a, b {
        return a + b
    }
    
    sayHello () "Sven"      // Prints 'Hello Sven'
    var two = add () 1, 1   // two == 2

You can even add it to an object and point to the same function like expressions:

    var a = void {
        func sayHello () name {
            print () "Hello " + name
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
            print () "Larger"
        }
        if cond == false {
            print () "Less or equal"
        }

Often it is possible to write it in a simpler way:

    print () .msg {
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

##For loop

Oup supports loops to create many objects of similar kind:  

    for i <- range () 0, 10 {
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
This is the only module public available that starts with a large capital.  
It contains all the standard functions for Oup.  

You can drop the 'oup.', unless you declare your own modules with same names.  
The evaluator searches for the functions in following order:

    previous declared
    parent expressions
    oup modules
    oup files in folder
    
Oup modules are named after verbs:

    oup.Convert     // Convert between data types.  
    oup.Calc        // Numbers, PI and stuff.  
    oup.Export
    oup.Group       // Combine collections.
    oup.Import  
    oup.Sort
    oup.Simulate

##Casting 
    
Casting of one variable to another is done by using the 'oup' module.  

    oup.Convert.ToNumber () "23"
    oup.Convert.ToString () 23
    oup.

##Multiple Sources

Oup supports the keyword 'return' at top level to emulate modules.  

    // a.oup
    var a = block () 0, 0, 100, 100
    return a
    
Use the '<-' assigment operator with '.oup' extension to evaluate the file.  
    
    // b.oup
    var b <- a.oup

