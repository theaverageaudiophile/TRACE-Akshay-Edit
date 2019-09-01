All buttons in the Basics group call the same form, which looks like this:

![Basics Form](https://github.com/Moosevellous/Trace/blob/master/img/frmBasics.JPG)

The form applies the formulas to the correct ranges of cells, depending on sheet type and applies styles if checkbox is selected.

# Logarithmics

## SPLSUM

Automatically inserts formulas for logarithmic addition of values using the usual formula

`SPLSUM=10*LOG(10^(SPL1/10) + 10^(SPL2/10) + 10(SPL3/10) ....)`

`Public Function SPLSUM(ParamArray rng1() As Variant) As Variant`

## SPLAV

Automatically inserts formulas for logarithmic average of values using the usual formula

`SPLSAV=10*LOG(10^(SPL1/10) + 10^(SPL2/10) + 10(SPL3/10) ....)-10*LOG(n)`

`where n=number of samples`

`Public Function SPLAV(ParamArray rng1() As Variant) As Variant`

## SPLMINUS

`SPLMINUS=10*LOG(10^(SPL1/10) - 10^(SPL2/10))`

`Public Function SPLMINUS(SPLtotal As Double, SPL2 As Double) As Variant`

# Conditionals

Both of the following functions use conditional operators contained in a string to determine if the numbers should be included in the set of values. 

| Symbol | Operator |
| --- | --- |
| "=" | Equals |
| ">=" | Greater Than or Equal To | 
| "<=" | Less Than or Equal To | 
| ">" | Greater Than | 
| "<" | Less Than | 

## SPLSUMIF

`Function SPLSUMIF(SumRange As Range, Condition As String, Optional ConditionRange As Variant)`

## SPLAVIF

`Function SPLAVIF(SumRange As Range, ConditionStr As String, Optional ConditionRange As Variant)`

# Misc. (still to come)

## Wavelength

## Speed of Sound

## Frequency Band Cutoff

Calculates the frequency band cutoff values for octave and third octave filters according to [ANSI S1.11](https://law.resource.org/pub/us/cfr/ibr/002/ansi.s1.11.2004.pdf)

`function GetFrequencyBandCutoff()`

`Function FrequencyBandCutoff(freq As String, Mode As String, Optional bandwidth As Double, Optional baseTen As Boolean)`