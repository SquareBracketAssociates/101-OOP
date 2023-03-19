## Solution of challenge yourself

	receiver: 3
	selector: +
	arguments: 4
	result: 7

	receiver: Date
	selector: today 
	arguments: _
	result: The date of today

	receiver: #('' 'World')
	selector: at:put:
	arguments: 1 and 'Hello'
	result: #('Hello' 'World')

	receiver: #(1 22 333)
	selector:	at:
	arguments: 2
	result: 22

	receiver: #(2 33 -4 67)
	selector: collect: 
	arguments: [ :each | each abs ]
	result: #(2 33 4 67)

	receiver: 25
	selector: @
	arguments: 50
	result: 25@50 (a point)


	receiver: the class SmalltalkInteger
	selector: maxVal
	arguments: _ 
	result: returns the largest small integer

	receiver: #(a b c d e f)
	selector: includesAll:
	arguments: #(f d b)
	result: true

	receiver: true
	selector: |
	arguments: false
	result: true

	receiver: Point 
	selector: selectors
	arguments: _
	result: a long arrays of selectors understood by the class Point 

> Float

> Symbol

> Array

> String

> Block

> Character

> Boolean

> SmallInteger

> Unary

> Unary

> Keyword-based

> Binary

> Binary

> Unary

> Keyword-based

> -2

> -2

> 32
anArray := #('first' 'second' 'third' 'fourth').
anArray at: 2


> 'second'

> #(4 9 100 9)

> 5

> -32

> true

is equivalent to 

x between: pt1 x and: pt2 y

is equivalent to 

#(a b c d e f) asSet intersection: #(f d b) asSet
     ifTrue: [....]
(x includes: y)
     ifTrue: [....]
	 
is equivalent to 


x isZero
     ifTrue: [....]
(x includes: y)
     ifTrue: [....]
    add: 56; 
    add: 33; 
    yourself

is equivalent to

OrderedCollection new
	    add: 56; 
	    add: 33; 
	    yourself

is equivalent to

3 + 4 + (2 * 2) + (2 * 3)

No changes

is equivalent to 

'http://www.pharo.org' asUrl retrieveContents