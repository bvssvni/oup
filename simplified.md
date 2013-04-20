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
    
##Ideas

1. Use brackets for sub-expressions.
2. Use '.' to refer to variable in sub-expression.
3. Use ':' for inheritance.
4. Use ':' for copying.
3. Operators repeats argument if it is only 1.
4. Only require space after operator.
5. Only require space in front of negative number.
4. Do not allow double-space around operators.
5. Only allow assignment as overridden values.
6. Require brackets for list.
7. Require brackets for dictionary.
8. Only one dictionary and list per node.
8. Allow list of dictionaries.
9. 'this' keyword.
4. Allow for-loops.
5. Use '0 -> 10' as notion for 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.
6. Use '10 <- 0' as notion for 9, 8, 7, 6, 5, 4, 3, 2, 1, 0.
7. Allow if-statements.
8. Allow function declarations.
9. Function return type by using ':'.
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
