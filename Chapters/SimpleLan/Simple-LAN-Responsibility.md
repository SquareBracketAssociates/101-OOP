## Improving the design

Often seasoned object-oriented programmers use the following description to convey good design principles: 

To be a good citizen an object should follow as much as possible the following rules:
- Be private. Never let somebody else play with its data.
- Be lazy. Let do other objects its job.
- Be focused. Do only one main task.

While these guidelines are generic, one of the main consequences is that this is the responsibility
of an object to provide a well-defined interface protecting itself from its clients. The other consequence
is that by delegating to other objects, an object concentrates on a single task and responsibility. We now
look at how such guidelines can help us to provide better objects in our example.

### Current situation

The interface of the packet class is weak. It just provides free access to its data. The main impact of
this weakness is the fact that the clients of the class `Packet` like the class `Workstation` rely on the internal coding
of the `Packet` class as shown in the first line of the following method. A workstation gets to know the internal structure of `Packet`.

```
Workstation >> accept: aPacket
    aPacket addressee = self name
        ifTrue: [ Transcript show: 'A packet is accepted by the Workstation ', self name asString ]
        ifFalse: [ super accept: aPacket ]
```

As a consequence, if the structure of the class `Packet` changes, the code of its clients would have
to change too. 
Generalizing such a bad practice leads to systems that are badly coupled and are really difficult to change to meet new requirements.

One of the solutions to these problems is to give the responsibility to the objects to create well-formed instances.
Several ways are possible:

- When possible, providing default values for instance variables is a good way to provide well-defined instances.
- It is also a good solution to propose a consistent and well-defined creation interface. For example, one can only provide an instance creation method that requires the mandatory value for the instance and forbids the creation of other instances.


### Enhancing the `Packet` API
Let us start with the class `Packet`.  This is the responsibility of a packet to say if the packet is addressed to a particular node or if it was sent
by a particular node.
- Define a method named `isAddressedTo: aNode` in 'testing' protocol that answers if a given packet is addressed to the specified node.
- Define a method named `isOriginatedFrom: aNode` in 'testing' protocol that answers if a given packet is originated from the specified node.
- Once these methods are defined, change the code of all the clients of the class `Packet` to use them.


### Instance initialization

Let us see how we can initialize instances. We investigate the two solutions for the `Packet` class.

Just remember that new is sent to classes (a class method) and that the message `initialize` is sent to 
instances (instance method). 

```language=smalltalk
    Packet >> initialize
        ....
```

In the `Packet` class, the only instance variable that can have a default value is contents. We can define it as follows: 

```language=smalltalk
    contents := 'no contents'
```

Ideally, if a LAN should contain a default trash node, the default address and originator should point
to it. We will implement this functionality in a future lesson. Implement first your own solution.

Note that with this solution it would be convenient to know if a packet contents is the default one or not. 
For this purpose, you can  provide the method `hasDefaultContents` that tests
that. 
You can implement it as :

```language=smalltalk
Packet >> hasDefaultContents
    ^ contents = 'no contents'

Packet >> initialize
    
    contents := 'no contents'
```

But you should apply the rule: 'Say only once' and define a new method that returns the default content
and use it as shown below:

```language=smalltalk
Packet >> defaultContents
    ^ 'no contents'
	
Packet >> initialize
    ...
    contents := self defaultContent
    ...
	
Packet >> hasDefaultContent

    ^ contents = self defaultContents
```

With this solution, we limit the knowledge to the internal coding of the default contents value to only
one method. This way changing it does not affect the clients nor the other part of the class.

### A question of creation responsibility

One of the problems with the previous approach for creating the nodes and the packets is the following: it
is the responsibility of the client of the objects to create them well-formed. For example, it is possible to
create a node without specifying a name. This is a disaster for our LAN system. The same problem occurs with the packet: it is possible to create a packet without an address nor contents.

We will find a solution to these problems.

!!! Exercise: new instance creation methods

Define a ''class'' method named `withName:` in the class `Node` (protocol 'instance creation') that creates a new node and assigns its name.

```language=smalltalk
Node class>>withName: aSymbol
    ....
```

Define a class method named `withName:connectedTo:` in the class Node (protocol 'instance creation') that creates a new node and assigns its name and the next node in the LAN.

```language=smalltalk
Node class>>withName: aSymbol connectedTo: aNode
    ....
```

The first method can simply invoke the second one.



### Exercise: Strengthening packet creation

We now apply the second approach by providing a better interface for creating packet.
For this purpose, we define a new creation method that requires a contents and an address.



Define a class methods named `send:to:` and `to:` in the class `Packet` (protocol 'instance creation') that creates a new `Packet` with a contents and an address.

```language=smalltalk
Packet class >> send: aString to: aSymbol
	....
Packet class >> to: aSymbol
	....
```


### Disabling default object creation
 
Imagine that we want to forbid the creation of non-well formed instances of these classes.
To do so, we will simply redefine the creation method `new` so that it will raise an error.

#### Exercise: Forbidding `new`

Rewrite the new method of the class `Node` and `Packet` as the following:

```
Node >> new
    self error: 'you should invoke the method... to create a...'
```

However, you have just introduced a problem: the instance creation methods you just wrote in previous exercise
will not work anymore, because they call `new`, and that calling results in an error! 
The solution is to rewrite them using `basicNew`.
This is for this reason that we should never override `basicNew`.
In fact, none of the methods starting with `basic` should be overridden.

In Pharo, there is a convention that all the methods starting with 'basic' should not be overridden. 
The method `basicNew` is the method responsible for always providing a newly created instance. 

```
Node class>>withName: aSymbol nextNode: aNode
    ^ self basicNew initialize name: aSymbol ; nextNode: aNode
```

### About forbidding `new`

Forbidding the message `new` is not always a good idea. 
Depending on the convention and flexibility we want to offer it may make your system more complex.
Imagine that you want to create packets automatically from different descriptions, it can be easier to simply create an empty packet and fill it up using setter methods. 

For complex objects, having methods with a large number of parameters can be tedious and it does not scale
especially when not all arguments are available. In such a case you would have to define many methods for each of the possible cases.

In such a case, it is better to favor a fluid interface based on the use of multiple setters.

```
MyObject new
	parameter1: '';
	parameter3: #whyNote;
	date: (Date fromString: '06/13/2015')
```


### Better `withName:` definition

Note that the previous definition of `withName: aSymbol nextNode: aNode` may break if a subclass specializes the `nextNode:` method and that it does not return (`self`) the instance anymore.

To protect ourselves from possible unexpected extension we add `yourself` that returns the receiver the first cascaded message (here name:), here the newly created instance.

```
Node class >> withName: aSymbol nextNode: aNode

    ^ self basicNew initialize name: aSymbol ; nextNode: aNode ; yourself
```

This definition is equivalent to that one:

```
Node class >> withName: aSymbol nextNode: aNode
    | instance |
    instance := self basicNew initialize.
    instance name: aSymbol. 
    instance nextNode: aNode. 
    ^ instance
```



### Conclusion

Object-oriented design cannot be systematically and automatically done. However, there are guidelines that help handling various pitfalls. 
Among such guidelines, the following ones are worth applying:
- Create well initialize objects.
- Offer adequate creation API.
- Expose APIs that protect the internal representation of objects.
- Avoid duplication logic.
