## Converter solutionThis chapter contains the solution of Chapter *@cha_converter@*.### Converting from Farhenheit to Celsius```TemperatureConverter >> convertFarhenheit: anInteger 
	"Convert anInteger from fahrenheit to celsius"
	
	^ ((anInteger - 32) / 1.8) ``````TemperatureConverter >> measureCount
	^ measures size```### Adding logging behavior```TemperatureConverter >> dates
	^ measures collect: [ :each | each key asDate ]``````TemperatureConverter >> measureFahrenheit: aTemp
	measures add: DateAndTime now -> (self convertFahrenheit: aTemp)	```