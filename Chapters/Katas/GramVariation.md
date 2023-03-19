## Variations around Isograms
	"'ALTRUISME' isIsogramDictionaryImplementation"
	| letters |
	letters := Dictionary new. 
	self do: [ :aChar |
			letters at: aChar 
				ifPresent: [^ false] 
				ifAbsent: [ letters at: aChar put: 1]. 
			].
	^ true
	"'ALTRUISME' isIsogramUsingBagImplementation"
	
	| bag |
	bag := Bag new. 
	self do: [ :each |
			bag add: each. 
			].
	^ bag sortedCounts first key = 1
	"'ALTRUISME' isIsogramFatestImplementation"
	1 to: self size -1 do: [ :ix |
	(self  
		findString: (self at: ix) asString
		startingAt: ix +1
		caseSensitive: false) ~~ 0 ifTrue: [ ^ false ]
	].
	^ true
	...

String >> isIsogramContainCharacter: aCharacter
	...
	"'ALTRUISME' isIsogramRecursiveImplementation"
	"'ALTRUISMEA' isIsogramRecursiveImplementation"
	^ self isEmpty
		ifTrue: [ true ] 
		ifFalse: [ self allButFirst isIsogramContainCharacter: self first ]
	
 	^ (self includes: aCharacter) 
			ifTrue: [ false ]
			ifFalse: [ self isIsogramRecursiveImplementation ]	 
   | i |
   i := 0.
   self asLowercase do: [ :char |
         | val |
         val := (char asInteger - 96).
         (val between: 1 and: 26) ifFalse: [ ^ false ].
         (i bitAt: val ) == 1 ifTrue: [ ^ false ].
         i := (i bitAt: val put: 1).
         ].
     ^ true
	lines := (ZnDefaultCharacterEncoder 
	  value: ZnCharacterEncoder latin1 
	  during: [
	    ZnClient new 
	      get: 'http://www.pallier.org/ressources/dicofr/liste.de.mots.francais.frgut.txt' ]) lines.

	bits := lines select: #isIsogramBitImplementation. 
	dicts := lines select: #isIsogramDictionaryImplementation. 
	bags := lines select: #isIsogramBagImplementation.
	strings := lines select: #isIsogramStringImplementation.
	sets := lines select: #isIsogramSetImplementation.
	recurs := lines select: #isIsogramRecursiveImplementation.
	{ bits . dicts .  bags . strings . sets} collect: #size. "#(23118 47674 47674 47668 47674)"
>>> 0
Time millisecondsToRun: ['PHRAO' isIsogramSetImplementation ] 
>>> 0

'ALTRUISME' isIsogramStringImplementation 

'ALTRUISME' isIsogramUsingBagImplementation

'ALTRUISME' isIsogramDictionaryImplementation

'ALTRUISME' isIsogramBit 
	"['ALTRUISME' isIsogramBit] bench '337,427 per second'"

   | i |
   i := 0.
   self do: [ :char |
         | val |
         val := (char asLowercase asInteger - 96).
         (val between: 1 and: 26) ifFalse: [ ^ false ].
         (i bitAt: val ) == 1 ifTrue: [ ^ false ].
         i bitAt: val put: 1
         ].
     ^ true