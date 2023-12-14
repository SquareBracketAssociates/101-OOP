## Variations 

This chapter uses the basic LAN and adds new classes and behavior. Doing so, the design is
extended to be more general and adaptive.

### From a ring to a star

Right now your LAN is a ring, the token has to pass through the nodes one by one and this is not possible to
send a packet to multiple nodes at once as this is the case in a star architecture.

#### Propose a solution to this problem. 
You can for example define a new node that (1) can be connected to several nodes and (2) broadcast a packet to all its nodes except the one from which it received the packet. 


### Handling loops

- When a packet is sent to an unknown node, it loops endlessly around the LAN. You will implement two solutions for this problem.

- Solution 1. The first obvious solution is to avoid that a node resends a packet if it is the originator of the packet that it is sent. Modify the accept: method of the class Node to implement such a functionality.

- Solution 2. The first solution is fragile because it relies on the fact that a packet is marked by its originator and that this node belongs to the LAN. A ‘bad’ node could pollute the network by originating packets with an anonymous name. Think about different solutions. Among the possible solutions, two are worth to be further analyzed:

-- 1. Each node keeps track of the packets it already received. When a packet already received is asked to be accepted again by the node, the packet is not sent again in the LAN. This solution implies that the packet can be uniquely identified. Their current representation does not allow that. We could imagine tagging the packet with a unique generated identifier. Moreover, each node would have to remember the identity of all the packets and there is no simple way to know when the identity of the treated node can be removed from the nodes.

-- 2. Each packet keeps track of the node it visited. Every time a packet arrives at a node, it is asked if it has already been here. This solution implies a modification of the communication between the nodes and the packet: the node must ask the status of the packet. This solution allows the construction of different packet semantics (one could imagine that packets are broadcasted to all the nodes, or have to be accepted twice). Moreover, once a packet is accepted, the references to the visited nodes are simply destroyed with the packet so there is no need to propagate this information among the nodes.

We propose you to implement the second solution so that the class `Packet` provides the following
interface (the new responsibilities are in bold).

```
Packet inherits from Object
Collaborators: Node
Responsibility:
	addressee returns the addressee of the node to which the packet is sent.
	contents describes the contents of the message sent.
	originator references the node that sent the packet.
	isAddressedTo: aNode answers if a given packet is addressed to the specified node. isOriginatedFrom: aNode answers if a given packet is originated from the specified node.
	isAcceptableBy: aNode answers if a packet is acceptable by a node
	hasBeenAcceptedBy: aNode tells a packet that it has been accepted by a given node.
```

New instance variable. A packet needs to keep track of the nodes it visited. Add a new instance variable
called `visitedNodes` in the class `Packet`. We want to collect the visited nodes in a set. Browse
the class Set and its superclass to find the function you need.

- Initialize the new instance variable. Modify the initialize methods of the class Packet so that the
visitedNodes instance variable is initialized with an empty set.
- Node Acceptation Methods. In a protocol named ‘node acceptation’, define the method `isAcceptableBy:`
and `hasBeenAcceptedBy:`.
- Write a test if your implementation works by sending a ‘bad’ node with a bad originator into the LAN.

### Broadcasting and multiple addresses

 Up to now, when a packet reaches a node it is addressed to, the packet is handled by the node and the
transmission of the packet is terminated (because is not sent to the next node in the network). 

In this exercise, we want you to provide facilities for broadcasting. If a node handles a packet that is broadcasted,
the packet must be sent to the next node in the LAN instead of terminating the connection. For example,
broadcasting makes it possible to save the contents of the same packet on different file servers of the LAN.
First try to solve this problem, and implement it afterwards.

In the current LAN, a packet only has one addressee. This exercise wants to add packets that have
multiple addresses. Propose a solution for this problem, and implement it afterward.

### Introduce links

Currently a node is connected to the following one by a direct reference. We would like to get a model that is closer to reality. For this a node should have a link object that connects two nodes. 

A link will have a `from` and `to` variables that respectively refers to the current node and its following node. A Node will now refer to link to send a packet.

Define some scenario as tests and introduce the class `LNLink`.

### Different documents

 Suppose we have several kinds of documents (ASCII and Postscript) and two kinds of LANPrinter in the
LAN (`LANASCIIPrinter` and `LANPostscriptPrinter`). We then want to make sure that every printer prints
the right kind of document. Propose a solution for this problem.

### Logging node

We want to add a logging facility: this means each time a packet is sent from a node, we want to identify the
node and the packet. Propose and implement a solution. 

Hint: introduce a new subclass of `Node` between `Node` and its subclasses and specialize the `send:` method.

### Automatic naming

The name of a node has to be specified by its creator.
We would like to have an automatic naming process that occurs when no names are specified. Note that the names should be unique. 
As a solution we propose you to use a counter, as this counter has to last over instance creations but still does not have any meaning for a particular node we use an instance variable of the class node.

Note that the NetworkManager could also be the perfect object to implement such a fonctionality. We
also would like that all the printer names start with Pr. Propose a solution.
