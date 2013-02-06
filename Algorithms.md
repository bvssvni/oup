#Algorithms

Oup does not allow nested paranthesis, which makes parsing faster and easier.  

##Groups

The parsing of Oup relies heavily on 'groups'.  
It makes translation from one programming language to another easier,  
since groups are not dependent on the string processing libraries of a specific language.  

A group is an object where members can swap places.  
The most efficient representation for text processing is a list of intervals:  

    0,10, 11,24, 25,30
    
Notice that the numbers always increases.  
Because a group is just a list of number, it does not need its own type in Oup.  

One benefit with groups is that each step can be unit tested.  
The algorithms necessary will be added to the 'Group' object in ![csharp-utils](https://github.com/bvssvni/csharp-utils).  

###And

Takes two groups and returns a group with members that are in both groups.  

###Or

Takes two groups and returns a group with members in both of them.  

###Except

Takes two groups and returns a group with members of the first that are not members of the second.  

###Interval index

Sometimes a group refer to intervals in another group.  
Here is how one gets the interval from a group using index 'i':

    func Interval () g, i {
        return g [] .a, .b {
            var a = i << 1
            var b = a + 1
        }
    }

##Group of Souce

Contains the entire source code of the file.  

    0, 5432

##Group of Lines

Contains the interval of the lines.  

    0, 10
    11, 25
    26, 40

##Token

A token is an object that tells about a piece of code:  

    start
    end
    line
    type

##List of Tokens

All tokens are pushed to a list such that even terms are 'names' and 'odd' terms are 'operators'.  
This list is used to create groups referring to a part of expression or source.  

##Reserved Keyword Groups

Reserved keywords such as 'var' or 'for' added to list of tokens,  
because they break the pattern <name> <op> <name> ...  
Instead, they are added to a group, one for each reserved keyword.  

    for
    var
    if
    func

##Sub-Expression Tree

There can only be one '{' and '}' operator per line.  
The tree is used to limit the context to branches of the tree.  

Sub-Expression Tree is based on Group of Lines.

##Dependency Tree

A variable can be assigned an expression, which has dependencies of other tokens.  

    var -> token -> expr -> group -> token -> expr -> group -> ...

Dependency Tree is based on List of Tokens.  

