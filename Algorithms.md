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

    var interval = g [] i << 1, g [] i << 1 + 1

##All Group

Contains the entire source code of the file.  

    0, 5432

##Lines Group

Contains the interval of the lines.  

##Token

A token is an object that tells about a piece of code:  

    start
    end
    line
    type

##Stack -> List -> Tree

Oup starts with pushing tokens on stacks, one at a time.  
There are 3 stacks:  

1. One stack for 'names'.  
2. One stack for 'operator'.  
3. One stack for precendence of the opereator.  



    1, 2 + 3 * 5, 6
    
    
