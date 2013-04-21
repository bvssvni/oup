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

##Range Up And Range Down

    0 -> 10     // Numbers from 0 to 9.
    10 <- 0     // Numbers from 9 to 0.

If 'x -> y' and x is bigger or equal to y, an empty range is returned.

##Ideas

1. Sub-expression are not sub nodes, they need to be placed in list.
2. Optional inheritance.
3. Function Declaration.
2. Sub-expression evaluated for each item in list.
2. Each item in list gets members.
3. '=' requires ':=' to be declared later, but overrides it.
2. '_index' to refer to current evaluated index.
3. '_value' to refer to the current evaluated value.
4. Use '_this' to refer to the current processed item.
5. Automatic static members.
4. Allow for-loops.
7. Allow if-statements.
8. Allow function declarations.
9. Function return type by using ':'.
10. Functions require arguments of base function to be passed.
11. List arguments can be passed left to '()' when calling function.
12. Choose between '.' or '[]' to access list item.
12. Reflection supported by using '[]' as value.
11. Declare functions inside variable body.
10. A function is a variable created by passing arguments.
11. No return from functions, using public members instead.
9. Allow modules.
10. Big first letter for public function.
11. Use ')(' to declare functions.

##Optional Inheritance

    a := .x, .y {
       x = 0
       y = 0
    }
    
    b : a = .y, .x
    
    c : a {
        x = 10
    }
    
'b' inherits 'a' but changes the order of the items in list.  
'c' inherits 'a' but changes the value of one of the sub-expressions.

##Function Declaration

    Length := .length )( x, y {
        length := Math.Sqrt () x * x + y * y
    }
    
    a := Length () 10, 20

##Declare Functions Inside Variable Body

    Complex () x , y {
        Re := x
    	Img := y
    	Multiply : Complex () b : Complex {
    		Re = this.Re * b.Re - this.Img * b.Img
    		Img = this.Re * b.Img + this.Img * b.Re
    	}
    	Add : Complex () b : Complex {
    		Re = this.Re + b.Re
    		Img = this.Img + b.Img
    	}
    }
    
    a := Complex () 0 , 1
    b := Complex () 10 , 10
    c := a.Multiply () b
    d := b.Add () a

##Challenges

1. Make it easy to parse.
2. Space around item separator.
3. Semantics of assignment.
