oup
===

Oup - A content generation programming language (under work).

##The Purpose of Oup

1. Optimize expressions.  
2. Emit objects to backend.  
3. Display editable values of objects in real-time.  
4. Allow the user to edit the values for rapid prototyping and debugging.  

##Why Expressions?

Expressions is particularly useful in applications where you can "poke" the data without concerning  
about the consequences. When an expression gets poked, Oup finds all expressions that depends on it
and updates their values.

    var a = 0
    var b = a + 1
    Print () b      // Prints '1'.
    a <- 3          // "Poke" a.
    Print () b      // Prints '4'.

In many programs that generate content, we want to do almost the same thing, over and over,  
with small modifications and see what happens.  
The result can be printed to a console, displayed on a webpage or rendered to an image.  

Oup can also let another editor poke the "roots" of expression and display changes in real-time.  

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

This is messy and easy to make mistakes, but in we can write this way:  

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

If we want to share variables shared between the two blocks, we can write like this:  

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
    ...
    a <- 20

We are "poking" 'a' to update all expressions depending on it.  

##Syntax

See 'Syntax.md'.  
