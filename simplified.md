#Simplified Syntax

##Declaration vs Assigmnet

    a := 0      // This is a declaration and assignment in one operation.
    a = 0       // This is an assigment that requires the variable to be declared.

##Required Space

    a:=0        // Space is required around operator.
    a := 0      // OK.
    
##Unary Subtraction

This is handled by requiring space around operator.

    a = 10 - -3
    
Ideas

1. Do not allow double-space around operators.
2. Use ')(' to declare functions.

Challenges

1. Make it easy to parse.
