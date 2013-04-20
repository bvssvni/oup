#Crazy Ideas

Oup is a programming language that is not evaluating its own expression.
It parses a source and calls methods on an interface.
The interface acts like a state machine does the evaluation.

##Interface

Here is the pseudo-code of the Oup interface:

    interface OupInterpreter {
        //      When parent id is -1, the variable must be added at top level.
        //      Returns the id of the variable created.
        //      Example:
        //          var a = 1, 2, 3
        //          var b = "hello"
        //          var c = 23.3049
        //          var d = a, b, c
        //          var e   // Empty list.
        id createVariable (parentId, name, list)
        
        //      Is called when a variable is assigned a new list.
        //      Example:
        //          var a = 1, 2, 3
        //          a = 4, 5, 6
        void overwriteVariable (id, list)
        
        //      An 'if' is a variable assigned to an expression
        //      that is evaluated to boolean value.
        //      Example:
        //          var a = 1
        //          if b = a > 0
        //          b {
        //              a = -1   // since b is true, we assign 2 to 'a'.
        //          }
        id createIf (parentId, name, list)
        
        bool evaluateIf (ifId)
        
        id createFunction (parentId, name, list)
        id createFor (parentId, name, list)
        
        //      Copy the content, including sub 
        void copy (destinationId, sourceId)
    }

##Examples

Oup's purpose is not to evaluate expressions,
but to structure data.

For example

    var a = 1 + 1
    
Is stored as a Oup list

    1 + 1
    
In a dictionary where the key is 'a'.
How this expression is evaluated depends on the use of the data and not of Oup.

Oup allows one variable to be copied from another:

    var a = 1 + 1
    var b : a
    
Here 'b' copies the list 'a'.

Each variable has also a dictionary.
In the dictionary the sub-expressions are stored.
When there is a '.' in front of a variable name, it means 'look up in sub-expression'.

    var a = .b {
        var b = 10
    }
    
When there is a '..' in front of a variable name, it means 'look up in parent-expression'.

    var a = 2
    var b = 3 {
        var c = ..a     // c is 2
    }
    
When there is a '...' in front of a variable name, it means 'look up in top-expression'.

    var a = 1
    var b = void {
        var c = void {
            var d = ...a
        }
    }

Oup allows values to be changed

    var a = 1
    a = 2               // a is now 2

When a variable is copied from another,
it also copies sub-expressions.

    var a = void {
        var b = 3
    }
    var c : a {
        b = 5           // c.b is now 5.
    }
    
If one attempts to assign a variable that is not declared,
an exception will be thrown.

    var a
    var c : a {
        b = 7           // ERROR: 'b' is not declared.
    }
    


##Oup List

An Oup list is a list with 'even' and 'odd' items.

The 'even' items either contain

1. Text.
2. Number.
3. Id.

The 'odd' items is just a 'code' of non-alphanumeric characters.

For example

    var a = 1 # 1
    
Is a valid Oup expression.

 
