## Electronic wallet solution
	"Add to the receiver, anInteger times a coin of value aNumber"

	bagCoins add: aCoin withOccurrences: anInteger 

	^ bagCoins occurrencesOf: aNumber
	"Return the value of the receiver by summing its constituents"
	| money |
	money := 0.
	bagCoins doWithOccurrences:
			[ :elem : occurrence | 
				money := money + ( elem * occurrence ) ].
	^ money
	"returns true when we can pay the amount of money"
	^ self money >= amountOfMoney
	"Returns the biggest coin with a value below anAmount. For example, {(3 -> 0.5) . (3 -> 0.2) . (5-> 0.1)} biggest -> 0.5"

	^ bagCoins sortedElements last key
	"Returns the biggest coin with a value below anAmount. For example, {(3 -> 0.5) . (3 -> 0.2) . (5-> 0.1)} biggestBelow: 0.40 -> 0.2"
	
	bagCoins doWithOccurrences: [ :elem :occurrences |
			anAmount > elem ifTrue: [ ^ elem ] ].
	^ 0
	"Add to the receiver a coin of value aNumber"
	
	bagCoins add: aNumber withOccurrences: 1 
	"Remove from the receiver a coin of value aNumber"
	
	bagCoins remove: aNumber ifAbsent: [] 
	"Returns a wallet with the largest coins to pay a certain amount and an empty wallet if this is not possible"
	| res |
	res := self class new.
	^ (self canPay: aValue)
		ifFalse: [ res ]
		ifTrue: [ self coinsFor: aValue into2: res ] 
	| accu |
	[ accu := accuWallet money.
	accu < aValue ]
		whileTrue: [
			| big |
			big := self biggest.
			[ big > ((aValue - accu) roundUpTo: 0.1) ] 
				whileTrue: [ big := self biggestBelow: big ].
			self removeCoin: big.
			accuWallet addCoin: big ].
	^ accuWallet 