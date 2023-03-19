## Some collection katas solutions
	"Returns true if the receiver is an isogram, i.e., a word using no repetitive character."
	"'pharo' isIsogramSet
	>>> true"
	"'phaoro' isIsogramSet
	>>> false"
	
	^ self size = self asSet size 
	"Returns true is the receiver is a pangram i.e., that it uses all the characters of a given alphabet."
	"'The quick brown fox jumps over the lazy dog' isPangramIn: 'abcdefghijklmnopqrstuvwxyz'
	>>> true"
	"'tata' isPangramIn: 'at'
	>>> true"

	alphabet do: [ :aChar |
		(self includes: aChar)
			ifFalse: [ ^ false ]
		].
	^ true
	"Returns true is the receiver is a pangram i.e., that it uses all the characters of a given alphabet."
	"'The quick brown fox jumps over the lazy dog' isEnglishPangram
	>>> true"
	"'The quick brown fox jumps over the dog' isEnglishPangram
	>>> false"

	^ self isPangramIn: 'abcdefghijklmnopqrstuvwxyz'
	"Return the first missing letter to make a pangram of the receiver in the context of a given alphabet. 
	Return '' otherwise"
	
	alphabet do: [ :aChar |
		(self includes: aChar)
			ifFalse: [ ^ aChar ]
		].
	^ ''
	
	| missing |
	missing := Set new. 
	alphabet do: [ :aChar |
		(self includes: aChar)
			ifFalse: [ missing add: aChar ] ].
	^ missing
	"Returns true whether the receiver is an palindrome.
	'anna' isPalindrome2
	>>> true
	'andna' isPalindrome2 
	>>> true
	'avdna' isPalindrome2 
	>>> false
	"
	1 
		to: self size//2 
		do: [ :i | (self at: i) = (self at: self size + 1 - i)
						ifFalse: [ ^false ]
				].
	^true