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

See 'Syntax.md'.  

##Lazy Evaluation

When using the '<-' operator, the variable is treated as a constant in expressions.  

    var a <- 5
    var b = a * 10
    
The only different from using '=' instead, is that you allow 'a' to be modified at run-time:

    var a = 5
    var b = a * 10
    
When declaring a variable in a function and returning it, the value is still editable,  
as long there is a 1:1 relationship between the returned value and the variable.  
    
    func myValue () void {
        var a = 5
        return a
    }
    
    var b = 10 * myValue () void

The evaluator uses groups to join the dependices of expressions.  
Each variable is tagged with a number, and by using group-oriented programming,  
a dependency is not listed more than once.  

##Randomness

Oup marks all functions that return a value that changes over time as 'random' functions.  
This property is traced through all dependices, so it will complain if an 'emitter' function is called  
with an argument that depends on 'random'.  

The reason for this is that when connected to a backend, the construction of a syntax tree and emitted  
objects must be identical on both sides.  
The same goes when an 'if' or 'for' statement is depending on 'random' in the condition.  
All variables and expressions in that block is then marked as 'random'.  

Notice that you can use pseudo-random functions without problems, since these are deterministic.  

##Communication With Backend

All functions in Oup that are linked to a backend is tagged with options that helps the evaluator.  
One of these is 'emitter' that tells that an object is created.  

When Oup is not connected to a backend, it will capture the emitted objects and dump them in raw format.  
This format is Oup code, so it can read itself.  
You can also write XML and JSON exporters in Oup.  

When Oup is connected with a backend, it will be able to "talk" to it.  
The trick is that the backend reads the Oup file and constructs a Oup syntax tree.  
The same happens in the Oup IDE, and Oup is designed to make this happen deterministically,  
so the same syntax tree is on both sides.  
One way to do this is sending an SHA1 key of the emitted object data to confirm.  
This is done by using the same technique as in Real Time Strategy multiplayer games.  

When the backend changes an object, for example the user changes a color and radius, 
it will send a pieace of Oup code through a channel with the 'id:value' of the object.  
This happens automatically using multi-threads on a human readable update frequence.  
A thread simply keeps a copy of each variable and checks if they have updated.  
The backend does not have to send data for loop, only like .5 seconds or so.  
For example:  

     4:255,0,0      // Parameters to a Color function.  
     6:15           // Value for the radius variable.  
     
The Oup IDE parses this text and looks up the variables and then present the values on the screen.  
The values changes color in the text editor to indicate that a change just happened.  

The communication can also happen the other way, by the user changing a value and the IDE sends  
this change over to the backend, which can choose to ignore it.  
The text editor is not updated until the backend sends a new notification about the update.  

Oup is designed for tools, such as level editors and not optimized for smooth game experience.  
The problem with games is that they create and destroy objects all the time,  
which can not be handled by the expression optimizer in Oup.  
Oup is a content generation programming language, where destroying objects never happens.  
You can compare it to painting where erasing is not possible without doing it all over.  

