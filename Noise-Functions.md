# Noise Functions Module
This module is at the heart of Trace, and does the most heavy lifting, computationally. It is expected that this section of Trace will have the most development as more functions are requested from acoustic engineers and implemented as needs arise.

## Air Absorption
`Private Function AirAbsorb(freq As String, Distance As Integer, temp As Integer)`

Air absorption varies over frequency with the following values (from 63Hz octave band)

`Absorption=Array(0.1,0.3,1.1,2.8,5,9,22.9,76.6) 'per km`

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

The button `Area Correction` applies the formula to all octave bands or one-third octave bands.

## Mech Elements
### ASHRAE Duct
### Flex Duct
### End Reflection Loss (ERL)

`Function GetERL(TerminationType As String, freq As String, DuctArea As Double)`

End Reflection Loss (ERL) happens as sound waves in the low end of the frequency spectrum exit a duct into a large room, some of that sound energy gets reflected back into the duct.

There are two types of duct terminations defined in the ASHRAE handbook, Chapter 48 _”Noise and Vibration Control”_:

**Flush**
Defined by the following formula

**Free Space**
Defined by the following formula

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
## Room Loss
### Classic
`Function GetRoomLoss(fstr As String, L As Double, W As Double, H As Double, roomType As String)`

**Values for alpha**

| Room Type | 63Hz | 125Hz | 250Hz | 500Hz | 1kHz | 2kHz | 4kHz |
|---|---|---|---|---|---|---|---|
| Live | 0.2 |  0.18 |  0.14 |  0.11 |  0.1 |  0.1 |  0.1 |  0.1 |  0.1 | 
| Av. Live   | 0.19 |  0.18 |  0.17 |  0.14 |  0.15 |  0.15 |  0.14 |  0.13 |  0.12 | 
| Average | 0.2 |  0.18 |  0.19 |  0.19 |  0.2 |  0.23 |  0.22 |  0.21 |  0.2 | 
| Av. Dead | 0.21 |  0.2 |  0.23 |  0.24 |  0.25 |  0.28 |  0.27 |  0.26 |  0.25 | 
| Dead | 0.22 |  0.2 |  0.28 |  0.3 |  0.4 |  0.47 |  0.45 |  0.44 |  0.45 | 

`alpha_av = ((L * W * alpha(bandIndex) * 2) + (L * H * alpha(bandIndex) * 2) + (W * H * alpha(bandIndex) * 2)) / S_total`

`Rc = (S_total * alpha(bandIndex)) / (1 - alpha_av)`

`GetRoomLoss = 10 * Application.WorksheetFunction.Log10(4 / Rc)`

### RT method

`Function GetRoomLossRT(fstr As String, L As Double, W As Double, H As Double, RT_Type As String)`

**Values for alpha**

| Room Type | 31.5Hz | 63Hz | 125Hz | 250Hz | 500Hz | 1kHz | 2kHz | 4kHz | 8Khz |
|---|---|---|---|---|---|---|---|---|---|
| <0.2sec | 0 |  0 |  0.21 |  0.277 |  0.331 |  0.385 |  0.435 |  0.446 |  0 | 
| 0.2 to 0.5sec | 0 |  0 |  0.125 |  0.138 |  0.183 |  0.233 |  0.288 |  0.296 |  0 | 
| 0.5 to 1sec | 0 |  0 |  0.109 |  0.112 |  0.137 |  0.18 |  0.214 |  0.225 |  0 | 
| 1 to 1.5sec | 0 |  0 |  0.057 |  0.056 |  0.058 |  0.069 |  0.08 |  0.082 |  0 | 
| 1.5 to 2sec | 0 |  0 |  0.053 |  0.053 |  0.06 |  0.08 |  0.095 |  0.1 |  0 | 
| >2sec | 0 |  0 |  0.063 |  0.052 |  0.036 |  0.041 |  0.035 |  0.04 |  0 | 

`alpha_av = ((L * W * alpha(bandIndex) * 2) + (L * H * alpha(bandIndex) * 2) + (W * H * alpha(bandIndex) * 2)) / S_total`

`Rc = (S_total * alpha(bandIndex)) / (1 - alpha_av)`

`GetRoomLoss = 10 * Application.WorksheetFunction.Log10(4 / Rc)`

### Room constant
## Direct / Reverberant Sum