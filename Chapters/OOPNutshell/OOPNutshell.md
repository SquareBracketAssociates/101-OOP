## Objects and classes
t := Turtle new. 
t := Turtle new. 
t turn: 90.
t go: 100.
t turn: 180. 
t go: 100.
t1 := Turtle new. 
t1 turn: 90.
t1 go: 100.
t1 turn: 180. 
t1 go: 100.
t2 := Turtle new.
t2 go: 100.
t2 turn: 40.
t2 go: 100.
	4 timesRepeat: [ self go: size; turn: 90 ]
   "Make the receiver draw a square of size s"

   self go: s; turn: 90; go: s; turn: 90.
   self go: s; turn: 90; go: s; turn: 90
c1 := Counter new.
c2 := Counter new.
c1 count.
>>> 0
c1 increment.
c1 increment.
c1 count. 
>>> 2
c2 count.
>>> 0
c2 increment.
c2 count. 
>>> 1
	instanceVariableNames: 'count'
	classVariableNames: ''
	package: 'LOOP'
	count := count + 1
	count := anInteger
	count := count - 1
   super printOn: aStream.
   aStream nextPutAll: ' with value: ', self count printString.
c2 := Counter new.
c2 count: 10.
c2 increment.
c2 increment.
c2 decrement.
	self increment. 
	self increment
dComics := MFDirectory new name: 'comics'.
dOldComics := MFDirectory new name: 'oldcomics'.
dComics addElement: dOldComics. 
dOldComics parent == dComics
>>> true
dOldComics inspect
dComics parent
>>> nil
dComics children includes: dOldComics. 
>>> true
dComics addElement: dManga. 
dComics children includes: dManga
>>> true
	instanceVariableNames: 'parent name files'
	classVariableNames: ''
	package: 'MyFS'
	files := OrderedCollection new
	aFile parent: self. 
	files add: aFile
	name := aString
	parent := aFile
	^ parent
	^ files
	aStream << name
el1 := MFDirectory new name: 'comics'.
el2 := MFDirectory new name: 'oldcomics'.
el1 addElement: el2.
el1 printString
>>> 'comics'
el2 printString
>>> 'oldcomics'
el1 := MFDirectory new name: 'comics'.
el2 := MFDirectory new name: 'oldcomics'.
el1 addElement: el2.
el1 >> printString.
>>>'comics'
el2 printString
>>> 'comics/oldcomics/'
	parent isNil 
		ifFalse: [ parent printOn: aStream ].
	aStream << name.
	aStream << '/'
el1 := MFFile new name: 'astroboy'; contents: 'The story of a boy turned into a robot that saved the world'.
dOldComics := MFDirectory new name: 'oldcomics'.
dOldComics addElement: el1. 
el1 printString.
>>>
'oldcomics/astroboy'
	instanceVariableNames: 'parent name contents'
	classVariableNames: ''
	package: 'MyFS'
	contents := ''
	name := aString
	parent := aFile
	^ parent
	contents := aString
	aStream << name
	parent isNil ifFalse: [ 
		parent printOn: aStream ].
	aStream << name
el1 := MFFile new name: 'astroboy'; contents: 'The story of a boy turned into a robot that saved the world'.
dOldComics := MFDirectory new name: 'oldcomics'.
dComics := MFDirectory new name: 'comics'.
dComics addElement: dOldComics.
dOldComics addElement: el1. 
el1 printString.
>>>
'comics/oldcomics/astroboy'
>>>
'comics/oldcomics/'
el := MFFile new name: 'babar'; contents: 'Babar et Celeste'.
el size = 'babar' size + 'Babar et Celeste' size.
>>> true
el := MFFile new name: 'babar'.
p2 := MFDirectory new name: 'oldcomics'.
p2 addElement: el. 
p2 size = 'oldcomics' size + 'babar' size + 2
>>> true
	^ contents size + name size
	| sum |
	sum := 0.
	files do: [ :each | sum := sum + each size ].
	sum := sum + name size.
	sum := sum + 2.
	^ sum
p := MFDirectory new name: 'comics'.
el1 := MFFile new name: 'babar'; contents: 'Babar et Celeste'.
p addElement: el1.
el2 := MFFile new name: 'astroboy'; contents: 'super cool robot'.
p addElement: el2.
(p search: 'Ba') includes: el1
>>> true
	^ '*', aString, '*' match: contents
	^ files select: [ :eachFile | eachFile search: aString ]
	| sum | 
	sum := 0.
	files do: [ :aFile | 
		aFile class = MFFile
			ifTrue: [ sum := sum + aFile name size + aFile contents size ].
		aFile class = MFDirectory
			ifTrue: [ 
				| fileSum |
				fileSum := 0.
				each files do: [:anInsideFile | fileSum := fileSum + anInsideFile name size + anInsideFile contents size ].
				sum := sum + fileSum + each name size + 2].
	^ sum	