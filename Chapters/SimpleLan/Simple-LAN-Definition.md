## A basic LAN application

The purpose of this mini project is to define a little network simulator. 
 If you understand well basic object-oriented concepts, you can skip this part of the book even if this is fun to code a little simulator and in particular its less guided extensions.
 
From an object-oriented point of view, it is really interesting because it  shows that objects encapsulate responsibilities and that inheritance is used to define incremental behavior.


You will define step by step an application that simulates a simple Local Area Network (LAN).  You will create several classes: `LNPacket`, `LNNode`, `LNWorkstation`, and `LNPrintServer`. We start with the simplest version of a LAN. In subsequent exercises, we will add new requirements and modify the proposed implementation to take them into account.

![An example of a LAN with packets.](figures/lan-simple width=80)


###  Creating the class `LNNode`

The class `LNNode` will be the root of all the entities that form a LAN: a printer, a server, and a computer. This class contains the common behavior for all nodes. As a network is defined as a linked list of nodes, a node should always know its next node. A node should be uniquely identifiable with a name. We represent the name of a node using a symbol (because symbols are unique in Pharo) and the next node using a node object. It is the node's responsibility to send and receive packets of information. In the next section, we will define the class that represents packets.

```
LNNode inherits from Object
Collaborators: LNNode and LNPacket
Responsibility:
	- name (aSymbol) - returns the name of the LNNode.
	- hasNextNode - tells if the receiver has a next node.
	- accept: aLNPacket - receives a LNPacket and process it. By default, it is sent to the next node.
	- send: aLNPacket - sends a LNPacket to the next node.
```


#### Exercise: Class creation
Create a new package `LAN`, and create a subclass of `Object` called `LNNode`, with two instance variables: `name` and `nextLNNode`.

#### Exercise: Accessors
Create accessors and mutators for the two instance variables. Document the mutators to inform users that the argument passed to `name:` should be a `Symbol`, and the arguments passed to `nextNode:` should be a `LNNode`. 
We can imagine the following test to validate such a simple behavior.
```
LNNode >> testName
	| node |
	node := LNNode new.
	node name: #PC1.
	self assert: node name equals: #PC1
```


#### Exercise: `hasNextNode`   
Define a method called `hasNextNode` that returns whether the receiver has a next LNNode or not. 
The following test should pass. 

```
LNNode >> testHasNextNode

	self deny: LNNode new hasNextNode 
```

#### Exercise: `printOn:`   
Create an instance method named `printOn:` that puts the class name and name variable on the argument `aStream`. Include my next node's name only if there is a next node. 

Hint: look at the method `printOn:` from previous exercises or other classes in the system, and consider that the instance variable `name` is a symbol and `nextNode` is a `LNNode`. The expected `printOn:` method behavior is described by the following code:


```
(LNNode new
   name: #LNNode1 ;
   nextLNNode: (LNNode new name: #PC1)) printString

'LNNode named: ''LNNode1'' connected to: PC1'
```


#### Exercise: sending/receiving
A `LNNode` has two basic messages to send and receive packets. 

When a packet is sent to a node, the node has to accept the packet, and send it on. Note that with this simple behavior, the packet can 
loop infinitely in the LAN. We will propose some solutions to this issue later. To implement this behavior, you should add a protocol `send-receive`, and implement the following two methods:

```
Node >> accept: aPacket
    "Having received aPacket, send it on. This is the default behavior. My subclasses may override me to do something special."
	
    self send: aPackage 
```


```
Node >> send: aPacket
     "Precondition: self has a nextNode"
     
	 self name trace.
	' sends a packet to: ' trace.
	 self nextNode name traceCr. 
	 self nextNode accept: aPacket.
```

Note that 
- `trace` displays in the transcript the result of sending the message `printOn:` to the receiver.
- `traceCr` has a similar behavior but adds a carriage return at the end. 


###  Creating the class `LNPacket`

A package is an object that represents a piece of information that is sent from nodes to nodes. So the responsibilities of this object are to allow us to define the originator of the packet emission, the address of the receiver and the contents.

```
LNPacket inherits from Object
Collaborators: LNNode
Responsibility:
	addressee - returns the name of the node to which the packet is sent.
	contents - describes the contents of the message sent.
	originator - references the node that sent the packet.
```

#### Exercise: defining class `LNPacket`

In the `LAN` package, create a subclass of `Object` called `LNPacket`, with three instance variables: `contents`, `addressee`, and `originator`. Create accessors and mutators for each of them in the `accessing` protocol. The addressee is represented as a symbol, the contents as a string and the originator has a reference to a `LNNode`.

#### Exercise: adding a `printOn:` method  
Define the method `printOn: aStream` that puts a textual representation of a LNPacket on its argument `aStream`.

#### Exercise: Adding `isAddressedTo:`
Define the method `isAddressedTo: aNode` which returns whether a packet is addressed to a given node.

###  Creating the class `LNWorkstation`

A workstation is the entry point for new packets onto the LAN network. It can emit packets to other workstations, printers
or file servers. Since it is kind of network `LNNode`, but provides additional behavior, we will define it a subclass of `LNNode`.
Thus, it inherits the instance variables and methods defined in `LNNode`. Moreover, a workstation has to process packets that are addressed to it.

```
LNWorkstation inherits from LNNode
Collaborators: LNNode, LNWorkstation and LNPacket
Responsibility: (the ones of LNNode)
	- send: aPacket - sends a LNPacket.
	- accept: aPacket - perform an action on LNPackets sent to the workstation (printing in the transcript). For the other
	packets just send them to the following nodes.
```

#### Exercise: Define `LNWorkstation`

In the package `LAN` create a subclass of `LNNode` called `LNWorkstation` without instance variables. 

#### Exercise: Redefining the method `accept:`

Define the method `accept: aLNPacket` so that if the workstation is the destination of the packet, the following message is written into 
the Transcript. Note that if the packets are not addressed to the workstation they are sent to the next LNNode of the current one.

```
(LNWorkstation new
    name: #Mac ;
    nextNode: (LNPrinter new name: #PC1))
          accept: (LNPacket new addressee: #Mac)

A LNPacket is accepted by the Workstation Mac
```

Hints: To implement the accept of a `LNPacket` not addressed to the workstation, you could copy and paste the code of the `LNNode` class. However this is a bad practice, decreasing the reuse of code and the ''Say it only once'' rule. It is better to invoke the default code that is currently overridden by using `super`.

#### Exercise: Defining the method `emit:`

Define the method `emit:` that is responsible for inserting packets in the network in the method protocol `send-receive`. In particular a packet should be marked with its originator and then sent.

```
LNWorkstation>>emit: aLNPacket
    "This is how LNPackets get inserted into the network.
    This is a likely method to be rewritten to permit
    LNPackets to be entered in various ways. Currently,
    I assume that someone else creates the LNPacket and
    passes it to me as an argument."
 
	"your code here"
```

###  Creating the class `LNPrinter`

With nodes and workstations, we provide only limited functionality of a real LAN. Of course, we would like to do something with the packets that are travelling around the LAN. Therefore, you will now create a class `LNPrinter`, a special node that when it receives packets addressed to it, prints them (on the Transcript). Implement the class `LNPrinter`.

```
LNPrinter inherits from LNNode
Collaborators: LNNode and LNPacket
Responsibility:
	- accept: aLNPacket - if the packet is addressed to the	printer, prints the packet contents else sends the packet
	to the following node.
	- print: aLNPacket - prints the contents of the packet (into the Transcript for example).
```

###  Simulating the LAN

Implement the following two methods on the class side of the class `LNNode`, in a protocol called `examples`. But take care: the code presented below has some bugs that you should find and fix!.

```
LNNode class >> simpleLan
  "Create a simple lan"
  "self simpleLan"

  | mac pc node1 node2 igPrinter |

  "create the nodes, workstations, printers and fileserver"
  mac := Workstation new name: #mac.
  pc := LNWorkstation new name: #pc.
  node1 := LNNode new name: #Node1.
  node2 := LNNode new name: #Node2.
  node3 := LNNode new name: #Node3.
  igPrinter := LNPrinter new name: #IGPrinter.

  "connect the different LNNodes."
  mac nextNode: node1.
  node1 nextNode: node2.
  node2 nextNode: igPrinter.
  igPrinter nextNode: node3.
  node3 nextNode: pc.
  pc nextNode: mac.

  "create a LNPacket and start simulation"
  packet := LNPacket new
            addressee: #IGPrinter;
            contents: 'This LNPacket travelled around to the printer IGPrinter.

  mac emit: LNPacket.
```

```
LNNode class >> anotherSimpleLan
   "create the nodes, workstations and printers"

   | mac pc node1 node2 igPrinter node3 packet |
   mac:= Workstation new name: #mac.
   pc := Workstation new name: #pc.
   node1 := LNNode new name: #Node1.
   node2 := LNNode new name: #Node2.
   node3 := LNNode new name: #Node3.
   igPrinter := LNPrinter new name: #IGPrinter.

   "connect the different LNNodes." 
   mac nextNode: node1.
   node1 nextNode: node2.
   node2 nextNode:igPrinter.
   igPrinter nextNode: node3.
   node3 nextLNNode: pc.
   pc nextNode: mac.

   "create a LNPacket and start simulation''
   packet := LNPacket new
             addressee: #anotherPrinter;
             contents: 'This packet travels around
             to the printer IGPrinter'.
   pc emit: packet.
```

As you will notice the system does not handle loops, so we will propose a solution to this problem in the future. To break the loop, use Command .

###  Create the class `FileServer`

We can create new extensions for our LAN.  Create the class `FileServer`, which is a special `LNNode` that
saves packets that are addressed to it. We can use a simple collection to keep the packet 


```
FileServer inherits from LNNode
Collaborators: LNNode and LNPacket
Responsibility:
	- accept: aLNPacket - if the LNPacket is addressed to the file server save it (Transcript trace) 
        else send the LNPacket to the following LNNode.
	- save: aLNPacket - save a LNPacket.
	- retrievePacketsFrom: aLNNode - returns the packets sent from a given node.
	- reset - clean the packets received.
```


###  Conclusion

You created a simple simulator of a local network. In the following chapters we will revisit such project to illustrate different points.

