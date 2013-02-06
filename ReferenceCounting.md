#Reference Counting

A variable in Oup has a reference counter.  
When the reference counter is set to 0,  
it decrements the reference counter of all variables in the expression list.  
Then the variable is released from memory.  
That means it is no longer possible to edit one of the dependices to make it update.  
This is a way to do garbage collection on expressions. 

Here I use prime "'" to note the difference between old and new object.  

    var a = 0       // a:1
    a' = 1          // a:0, old object is released.
    
All reference counters on right side is increased before left side decreases.  
This is to prevent old objects from becoming released when they are expanded:  

    var a = 0       // a:1
    a' = a + 1      // a:1, old object is not released.

One way to tell that an object is no longer useful, is by setting it to 'void':  

    var a = 0       // a:1
    a' = void       // a:0, old object is released.
    
##Abuse Of Expressions

Expanding an expression in a for-loop can result slow execution and large memory usage.  
This happens because Oup adds new nodes to the expression tree.  

    var a = 0
    for i <- Range () 0, 10000 {
        a += 1      // Not good. At all.
    }
    
Instead, one can update the value, but this will overwrite the old value of the expression:  

    var a = 0
    for i <- Range () 0, 10000 {
        a <- a + 1
    }
    
##Reference Counting In Functions
    
When a function returns, all reference counters of variables created in the scope  
of the function will be decremented:  

    func sayHello () void {
        var msg = "Hello"     // msg:1
        Print () msg          // msg:1
    }                         // msg:0
    
A return statement increases the reference counters of the variables in expression list:  

    func Add () {
        var c = 0             // msg:1
        return c              // msg:2
    }                         // msg:1
    
If the returned expression from a function is not assigned, the reference counters will be decremented:

    Add ()                    // msg:0
    
