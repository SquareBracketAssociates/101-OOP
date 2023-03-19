## Tests, tests and tests
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'MySetTest'
	| s |
	s := Set new. 
	self assert: s isEmpty.
	s add: $A.
	self assert: s size equals: 1.
	s add: $A.
	self assert: s size equals: 1.
development_ movement, and any software developer concerned with improving
time._ If you never write a bug, and if your code will never be changed in the
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'MySetTest'
	| full |
	full := Set with: 5 with: 6.
	self assert: (full includes: 5).
	self assert: (full includes: 6)
tests \(t\)_ . The test methods will be flagged green or red,
	| empty full |
	empty := Set new.
	full := Set with: 5 with: 6.
	self assert: (empty occurrencesOf: 0) equals: 0.
	self assert: (full occurrencesOf: 5) equals: 1.
	full add: 5.
	self assert: (full occurrencesOf: 5) equals: 1
	| full |
	full := Set with: 5 with: 6.
	full remove: 5.
	self assert: (full includes: 6).
	self deny: (full includes: 5)
	"self run: #testRemove"
	| empty full |
	empty := Set new.
	full := Set with: 5 with: 6.
	full remove: 5.
	self assert: (full includes: 6).
	self deny: (full includes: 5)
	| empty full |
	empty := Set new.
	full := Set with: 5 with: 6.
	full remove: 5.
	self assert: (full includes: 7).
	self deny: (full includes: 5)

MyExampleSetTest debug: #testRemove
self assert: (empty occurrencesOf: 0) = 0.
	| empty |
	self should: [ empty at: 5 ] raise: Error.
	self should: [ empty at: 5 put: #zork ] raise: Error
>>> 1 run, 1 passed, 0 failed, 0 errors
>>> 5 run, 5 passed, 0 failed, 0 errors
	instanceVariableNames: 'full empty'
	classVariableNames: ''
	package: 'MySetTest'
	empty := Set new.
	full := Set with: 5 with: 6
	self assert: (full includes: 5).
	self assert: (full includes: 6)
	self assert: (empty occurrencesOf: 0) equals: 0.
	self assert: (full occurrencesOf: 5) equals: 1.
	full add: 5.
	self assert: (full occurrencesOf: 5) equals: 1
	full remove: 5.
	self assert: (full includes: 6).
	self deny: (full includes: 5)