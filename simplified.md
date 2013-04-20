#Simplified Syntax

##Declaration vs Assigmnet

    a := 0      // This is a declaration and assignment in one operation.
    a = 0       // This is an assigment that requires the variable to be declared.

##Required Space

    a:=0        // Error: Space is required after operator.
    a:= 0       // OK.
    
##Unary Subtraction

This is handled by requiring space after operator.

    a := 10 - -3
    
##Brackets For Sub-Expression

    a := {
        x := 0
        y := 0
    }
    
##Sub-Expression

    a := .x, .y {
        x := 0
        y := 0
    }
    
##Inheritance/Copy

    a : b       // Inherits all members from b.
    
##Automatic Operator Overloading

    a := 1, 2, 3, 4
    b = a * 4   // Multiplies each item in list.

##Assignment As Expression Input

    a := {
        x := 0
        x2 := x * x
    }
    b : a {
        x = 4
    }

##Referring To Parent

    a := 0, 1, 2 {
        x := this [] 0
    }

##Range Up And Range Down

    0 -> 10     // Numbers from 0 to 9.
    10 <- 0     // Numbers from 9 to 0.

If 'x -> y' and x is bigger or equal to y, an empty range is returned.

##Ideas

4. Allow for-loops.
7. Allow if-statements.
8. Allow function declarations.
9. Function return type by using ':'.
10. A function is a variable created by passing arguments.
11. No return from functions, using public members instead.
9. Allow modules.
10. Big first letter for public function.
11. Use ')(' to declare functions.

##Only Allow Assignment As Overriden Values

    a := {
        x := 0
        y := x
    }
    
    b : a {
        x = 10
    }

Challenges

1. Make it easy to parse.
2. Space around item separator.
3. Semantics of assignment.
