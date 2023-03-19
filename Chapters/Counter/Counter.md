## Developing a simple counter
counter := Counter new.
counter increment; increment.
counter decrement.
counter count = 1
   instanceVariableNames: 'count'
   classVariableNames: ''
   package: 'MyCounter'
Its API is 
- decrement, increment
- count
Its creation API is message withValue: 
   "Return the current value of the receiver"
   ^ count
>>> nil
c := Counter new count: 7.
c count
>>> 7
   instanceVariableNames: ''
   classVariableNames: ''
   package: 'MyCounter'
   | c |
   c := Counter new.
   c count: 7.
   self assert: c count equals: 7
	location: 'http://smalltalkhub.com/mc/PharoMooc/Counter/main'
	user: ''
	password: ''
	location: 'http://smalltalkhub.com/mc/YourAccount/YourProject/main'
	user: 'YourAccountID'
	password: 'YourAccountPassword'
   | c |
   c := Counter new.
   c count: 0 ; increment; increment.
   self assert: c count equals: 2
   count := count + 1
   count := count - 1
>>> nil
   self assert: Counter new count equals: 0
  "set the initial value of the value to 0"
  
  count := 0
>>> 2
	self 
		assert: Counter new increment; increment; count 
		equals: 2
>>> a Counter
   super printOn: aStream.
   aStream nextPutAll: ' with value: ', self count printString.