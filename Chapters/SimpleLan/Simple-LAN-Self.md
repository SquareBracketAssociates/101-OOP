## Fundamentals on Self and Super


This lesson is a reminder about `self` and `super`.

### self

Let us have a look at `self`. When the following message is executed: 

```
aWorkstation emit: (Packet new)`
```

- The system starts to look up the method `emit:` in the class of the message receiver: `Workstation`.
- Since this class defines a method `emit:`, the method lookup stops and this method is returned.
- The returned method executed with self bound to aWorkstation and aPacket bound to the newly created `Packet` instance.

Following is the code of the `emit:` method:
```
Workstation >> emit: aPacket
   aPacket originator: self.
   self send: aPacket
```

- It sends first the message `originator:` to an instance of class Packet with as argument `self` which represents the receiver of `emit:` method. The same process occurs. The method `originator:` is looked up in the class `Packet`. As the class `Packet` defines a method named `originator:`, the method lookup stops and the method is returned and executed. As shown below the body of this method is to assign the value of the first argument (aNode) to the instance variable originator. Assignment is one of the few constructs of Pharo. It is not implemented as a message sent but handled by the compiler. So no more message sends are performed for this part of `originator:`.
```
Packet >> originator: aNode
    originator := aNode
```
- In the second line of the method `emit:`, the message `send: aPacket` is sent to `self`. `self` represents the instance that receives the `emit:` message. The semantics of self specifies that the method lookup should start in the class of the message receiver: Here `Workstation`. Since there is no method `send:` defined on the class `Workstation`, the method lookup continues in the superclass of `Workstation`: class `Node`. `Node` implements `send:`, so the method lookup stops: it is returned and executed `send:`

```
Node >> send: thePacket
    self nextNode accept: thePacket
```

### super

Now we present the difference between the use of `self` and `super`. `self` and `super` are both pseudovariables
that are set by the system at runtime. They both represent the receiver of the message
being executed. However, there is no use to pass super as method argument, `self` is enough for this.

The main difference between `self` and `super` is their semantics regarding method lookup.
- The semantics of `self` is to start the method lookup into the class of the message receiver and to continue in its superclasses.
- The semantics of super is to start the method lookup into the superclass of class of the method using super and to continue in its superclasses. 

Take care the semantics is NOT to start the method lookup into the superclass of the receiver class, the system would loop with such a definition. Using `super` to invoke a method allows one to invoke
overridden methods.

Let us illustrate with the following expression: the message `accept:` is sent to an instance of `Workstation`.

```
aWorkstation accept: (Packet new addressee: #Mac)
```

As explained before the method is looked up into the class of the receiver, here `Workstation`. The
method being defined into this class, the method lookup stops, the method is returned and the method is executed.

```
Workstation >> accept: aPacket
    (aPacket addressee = self name)
       ifTrue: [ Transcript show: 'Packet accepted', self name asString ]
       ifFalse: [ super accept: aPacket ]
```

Imagine that the test returnsd false. The following expression is then executed:

```
super accept: aPacket
```

- The method `accept:` is looked up in the superclass of the class in which the method using `super` is used. Here the method using `super`is defined in `Workstation` so the lookup starts in the superclass of `Workstation`: class `Node`. The following code is executed following the rule explained before.


```
Node>>accept: aPacket
    self hasNextNode
        ifTrue: [ self send: aPacket ]
```

### Remark
The previous example does not show well the vicious point in the super semantics: the
method look into the superclass of class of the method using `super` and not in the superclass of the receiver class.
Some (average) books use this wrong definition about super. 

Do the following exercise to prove yourself that you understand well the nuance. Imagine now that we define a subclass of `Workstation` called `AnotherWorkstation` and that this class does NOT defined a method `accept:`. Simulate the lookup  the following expression with both semantics:

```
anAnotherWorkstation accept: (Packet new addressee: #Mac)
```

Do you see that the lookup loop?

### Conclusion

We illustrated how `self` and `super` work. While both refer to the receiver. The semantics of `self` is to start the method lookup into the class of the message receiver and to continue in its superclasses.
The semantics of super is to start the method lookup into the superclass of class of the method using super and to continue in its superclasses. 