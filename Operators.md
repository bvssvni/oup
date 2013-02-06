
#Operators

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
