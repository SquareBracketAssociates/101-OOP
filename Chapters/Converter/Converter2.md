## Temperature Logger \(under writing\)
    instanceVariableNames: ''
    classVariableNames: ''
    package: 'TemperatureLogger'

    | office |
    office := TemperatureLogger new location: 'Office'.
    "Perform two measures that are logged"
    office recordTemperature: 19.
    office recordTemperature: 21.

    "We got effectively two measures"
    self assert: office recordCount equals: 2.
    instanceVariableNames: 'location measures'
    classVariableNames: ''
    package: 'Converter'
    location := 'Home'.
    measures := OrderedCollection new

    location := aNumber
    "Add the temperature to the list of recorded measures"

    measures add: aNumber

    ...

    | office |
    office := TemperatureLogger new location: 'Office'.
    office recordTemperature: 19.
    office recordTemperature: 18.
    self assert: office latestRecord equals: 18.
    office recordTemperature: 21.
    self assert: office latestRecord equals: 21.

    ^ self addLast: newObject

    ^ measures last

    | office |
    office := TemperatureLogger new location: 'Office'.
    office recordTemperature: 16.
    office recordTemperature: 17.
    office recordTemperature: 19.
    office recordTemperature: 18.
    office recordTemperature: 21.
    self assert: (office latestRecords: 3) equals: #(21 18 19) asOrderedCollection

    | records reversed |
    records := OrderedCollection new.
    reversed := measures reversed.
    1 to: aNumber do: [ :each |
            records add: ( reversed at: each ) ].
    ^ records
    | records |
    records := OrderedCollection new.
    1 to: aNumber do: [:i | records add: (measures at: i)  ].
    ^ records

    | office |
    office := TemperatureLogger new location: 'Office'.
    office recordTemperature: 19.
    office recordTemperature: 21.
    office recordTemperature: 17.

    self assert: office average equals: 19
   ...

    ^ measures size

        | sum |
        sum := 0.
        measures do: [ :aMeasure | sum := sum + aMeasure ].
        ^ sum / measures size

    ^ measures average
    ^ self sum / self size

    | office |
    office := TemperatureConverter new location: 'Office'.
    "perform two measures that are logged"
    office measureCelsius: 19.
    office measureCelsius: 21.

    "We got effectively two measures"
    self assert: office measureCount = 2.

    "All the measures were done today"
    self assert: office dates equals: {Date today . Date today} asOrderedCollection.

    "We logged the correct temperature"
    self assert: office temperatures equals: { 19 . 21 } asOrderedCollection
    measures add: DateAndTime now -> aTemp
    ^ measures size
    ^ measures collect: [ :each | each value ]
    ^ measures collect: [ :each | each key asDate ]
    ^ measures collect: [ :each | each key asDate ]

    | office home |
    office := TemperatureConverter new location: 'office'.
    office measureFahrenheit: 19.
    office measureFahrenheit: 21.
    self assert: office location equals: 'office'.
    self assert: office measureCount equals: 2.
    home := TemperatureConverter new location: 'home'.
    home measureFahrenheit: 22.
    home measureFahrenheit: 22.
    home measureFahrenheit: 22.
    self assert: home location equals: 'home'.
    self assert: home measureCount equals: 3.
    instanceVariableNames: ''
    classVariableNames: ''
    package: 'Converter'

    | converter |
    converter := TemperatureConverter new.
    self assert: ((converter convertCelsius: 30) = 86.0)

    | converter |
    converter := TemperatureConverter new.
    self assert: (converter convertCelsius: 30) equals: 86.0
    "Convert anInteger from celsius to fahrenheit"

    ^ ((anInteger * 1.8) + 32)

    | converter |
    converter := TemperatureConverter new.
    self assert: (converter convertCelsius: 30) equals: 86.0

    | converter |
    converter := TemperatureConverter new.
    self assert: ((converter convertFarhenheit: 86) equals: 30.0).
    self assert: ((converter convertFarhenheit: 50) equals: 10)
    "Convert anInteger from fahrenheit to celsius"

    ^ ((anInteger - 32) / 1.8)
> false
> 0.30000000000000004
> true
(0.1 + 0.2) closeTo: 0.3 precision: 0.001
> true
> 11.11111111111111
> true
> true

    | converter |
    converter := TemperatureConverter new.
    self assert: ((converter convertFarhenheit: 86) closeTo: 30.0 precision: 0.1).
    self assert: ((converter convertFarhenheit: 50) closeTo: 10 precision: 0.1)
>11.11111111111111
>11.1
> { 50->10.0.
    52->11.1.
    54->12.2.
    56->13.3.
    58->14.4.
    60->15.6.
    62->16.7.
    64->17.8.
    66->18.9.
    68->20.0.
    70->21.1}
p1 := 50 -> 10.0.
p1 key
> 50
p1 value
> 10.0

    | converter results expectedCelsius |
    converter := TemperatureConverter new.
    results := (converter convertFarhenheitFrom: 50 to: 70 by: 2).
    expectedCelsius := #(10.0 11.1 12.2 13.3 14.4 15.5 16.6 17.7 18.8 20.0 21.1).

    results with: expectedCelsius
        do: [ :res :cel | res value closeTo: cel ]
    "Returns a collection of pairs (f, c) for all the farhenheit temperatures from a low to an high temperature"

    ^ (low to: high by: step)
        collect: [ :f | f -> (self convertFarhenheit: f) ]

    | converter |
    converter := TemperatureConverter new location: 'Lille'.
    converter measureFahrenheit: 86.
    converter measureFahrenheit: 50.
    self assert: converter measureCount equals: 2.
    self assert: converter dates
        equals: {Date today . Date today} asOrderedCollection.
    self assert: converter temperatures
        equals: { converter convertFahrenheit: 86 .
                converter convertFahrenheit: 50 } asOrderedCollection
    measures add: DateAndTime now -> (self convertFahrenheit: aTemp)