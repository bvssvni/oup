#Operators

Oup relies heavily on the use of infix operators.  

##List Operator Overloading

Because all variables are lists, the operators are designed to take advantage of this.  
Some operators like '+' and '*' behave in the following manner:  

When both arguments got more than one item, they need to have an equivalent amount of items.  

    var a = .b + .c {
        var b = 1, 2
        var c = 3, 4
    }
    
    var a = .b + .c {       // Error: Not equal amount of items.  
        var b = 1, 2
        var c = 3, 4, 7
    }
    
When one argument got only one item, the operation is repeated for each item in list with more items:  

    var a = .b * .c {       // Returns 2, 4, 6
        var b = 2
        var c = 1, 2, 3
    }

If you want to sum the items in a list, you can use the 'Sum' function:

    var a = Sum () 1, 2, 3, 4
    
When writing a list, the operators will happen on the individual location in the list:

    var a = 1, 2, 3 + 5, 6, 7   // Returns 1, 2, 8, 6, 7
    
To add two lists, you can use sub-expressions:

    var a = .b + .c {
        var b = ...
        var c = ...
    }

##Not vs Except

In Oup, there are no unary operators beside functions.
There is no 'Not' operator like '!' in C languages.
Instead, Oup uses '\\' which equals 'And Not' or 'Except' in everyday speech.

'\\' has lower precedence than '||', which means you can write more readable conditions  
by putting the '\\' operators to the right:

    C:
        if ((a || b) && !c) {
            ...
        }
        
    Oup:
        // a and b, except c
        if a || b   \\ c {
            ...
        } 

##Minus Sign

When there is a minus sign '-' after a non-numeric followed by a digit, like

    1+-2
    
or followed by an 'e' at a end of a word that starts with a digit or '-', like

    3e-8
    -3e-8

the minus sign is assumed to belong to a number.  

##Precedence

The precedence of operators in ascending order:

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
    
    <<      Bitshift left. 
    >>      Bitshift right.
    
    \       Bitwise Except.
    \\      Boolean Except.
    \\\     Group Except.
    
    +       Add.
    -       Subtract.
    %       Modulus.
            Example: var a = a * 2 ; 1  
    |       Bitwise Or.
    ||      Boolean Or.
    |||     Group Or.
            
    *       Multiply.
    /       Divide.
    &       Bitwise And.
    &&      Boolean And.
    &&&     Group And.
            
    ..      Creates a range of numbers.
            Example: 1..10
    "       Reads the rest of the line as string, or until it matches another ".  
        \"      Reads " to the current reading string.  
    '       Reads the next character as Char followed by a '.  
