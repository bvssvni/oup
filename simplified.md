#Simplified Syntax

Decided

    =   Assign, throws exception if variable is not declared.
    :=  Declare variable, throws exception if variable name is declared.
    ()  Function call, throws exception if left argument is not a name.
    ,   Item separator
    +=  Add right value to each item in list
    *=  Multiply right value with each item in list
    /=  Divide right value with each item in list
    -=  Subtract right value with each item in list
    []  Takes from list using right arguments as indices.
    
##Required Space

    a:=0        // Space is required around operator.
    a := 0      // OK.
    
##Unary Subtraction

This is handled by requiring space around operator.

    a = 10 - -3
    
Ideas

    )(  Declare function

Challenges

