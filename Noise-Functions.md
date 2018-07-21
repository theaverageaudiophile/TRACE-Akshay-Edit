# Noise Functions Module
This module is at the heart of Trace, and does the most heavy lifting, computationally. It is expected that this section of Trace will have the most development as more functions are requested from acoustic engineers and implemented as needs arise.

## Air Absorption
`Private Function AirAbsorb(freq As String, Distance As Integer, temp As Integer)`

Air absorption varies over frequency with the following values (from 63Hz octave band)

`Absorption=Array(0.1,0.3,1.1,2.8,5,9,22.9,76.6) 'per km

Absorption is assumed to be a loss and is therefore input as a negative value.

## Distance Attenuation
### Point Source
`10*log(Q/4*PI*(R^2))`

### Line Source
`10*log(Q/2*PI*R)`

### Plane Source
TBD

### Area correction
The correction for area is:
`10*log(A)`

The button `Area correction` applies the formula to all octave bands or one-third octave bands.

## Mech Elements
### ASHRAE Duct
### Flex Duct
### End Reflection Loss (ERL)

`Function GetERL(TerminationType As String, freq As String, DuctArea As Double)`

freq is converted to a value using the Trace function `freqStr2Num(freq)`

The formula for End reflection loss is given in the ASHRAE Handbook:

`GetERL = -10*log10(1 + ((A1 + c0) / (f * dia * Pi)) ^ A2)`

From ASHRAE tables:

|   | Flush | Free |
|---|---|---|
|A1 | 0.7 | 1 |
|A2 | 2 | 2 |

### Regenerated noise
### Elbow / Bend
### Duct Split
### Silencer
### Acoustic Louvres

## Direct / Reverberant Sum