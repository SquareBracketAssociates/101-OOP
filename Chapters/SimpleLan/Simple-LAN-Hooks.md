## About hooks

In this chapter you will learn how to introduce hooks and template methods to favor extensibility. 
First we look at the current situation and introduce changes step by steps.


### Current situation

The solution proposed for printing a Node just uses the default instance printing protocol of Pharo.

```
> (Node withName: #Node1 connectedTo: (Node new name: #PC1)) printString
a Node
```

We should improve it. 

#### Specializing the`printOn:` hook

This is simple we can specialize the default Pharo behavior by redefining the method `printOn:`
A straightforward way to implement the `printOn:` method on the class Node is the following code:

```
Node >> printOn: aStream
	aStream nextPutAll: ’Node named: ’, self name asString.
	self hasNextNode
		ifTrue: [ aStream nextPutAll: ’ connected to: ’, self nextNode name ]
```
Note that with such an implementation the printing of all kinds of nodes is however the same.


### Defining a new hook

To have a nicer description of a given LAN we would like that depending on the
specific class of node we obtain a specific printing like the following ones:

```
> (Workstation withName: #Mac connectedTo: (LanPrinter withName:
#PC1) printString
Workstation Mac connected to Printer PC1
```
```
> (LanPrinter withName: #Pr1 connectedTo: (Node withName: #N1)
printString
Printer Pr1 connected to Node N1
```

We could redefine the method `printOn:` in each of the subclasses but this would introduce some duplicated code.


Define the method `kindName` that returns a string representing the name of the type of node in the
‘printing’ protocol. This method should be defined in `Node` and all its subclasses.

```
> (LanPrinter withName: #PC1) kindName
‘Printer’
```

```
> (Node withName: #N1) kindName
‘Node’
```

#### Define the method `simplePrintString` on the class `Node` to provide more information about a node as
show below:

```
> (Workstation withName: #Mac connectedTo: (LanPrinter withName:
#PC1)) simplePrintString
‘Workstation Mac’
```

```
> (LanPrinter withName: #PC1) simplePrintString
‘Printer PC1’
```

Then modify the `printOn:` method of the class `Node` to produce the following output:

```
> (self withName: #Mac connectedTo: (LanPrinter new name:
#PC1))
‘Node Mac connected to Printer PC1’
```

### About hook and template

The method `kindName` is called a hook method. 
This reflects the fact that it allows the subclasses to specialize the behavior of the superclass, here the printing of all the different kinds of nodes.
The method `simplePrintString`, even if in our case is rather simple, is called a template method. This name
reflects the fact that the method specifies the context in which hook methods will be called and how they
will fit into the template method to produce the expected result.

Note that for abstract classes hook methods can be abstract too, one other case the hook method can
propose a default behavior.

The Pharo class library contains a lot of such hooks that allows an easy customization of the proposed
behavior. The proposed requirement already exists in the system.

### Follow up exercises

Study the method `printOn:` on the class `Object`. Check its implementors and senders.
