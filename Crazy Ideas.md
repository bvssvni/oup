#Crazy Ideas

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

    var a = void
    var c : a {
        b = 7           // ERROR: 'b' is not declared.
    }
    


##Oup List

An Oup list is a list with 'even' and 'odd' items.

The 'even' items either contain

1. Text.
2. Number.
3. Reference to another Oup list.
4. 'void' which is just a dummy to be ignored.

The 'odd' items is just a 'code' of non-alphanumeric characters.

For example

    var a = 1 # 1
    
Is a valid Oup expression.

