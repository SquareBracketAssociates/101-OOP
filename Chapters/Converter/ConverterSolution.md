## Converter solution
	"Convert anInteger from fahrenheit to celsius"
	
	^ ((anInteger - 32) / 1.8) 
	^ measures size
	^ measures collect: [ :each | each key asDate ]
	measures add: DateAndTime now -> (self convertFahrenheit: aTemp)	