oup
===

Oup - A content generation programming language (under work).

##The Purpose of Oup

Oup is not a real programming language, it is fictional.  
It functions as an toy model for generating ideas in the following areas:  

1. Experiment with expression generation.  
2. Emit objects to standard output, graphics etc.  
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

##Syntax

See 'Syntax.md'.  
