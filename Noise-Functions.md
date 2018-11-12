# Noise Functions Module
This module is at the heart of Trace, and does the most heavy lifting, computationally. It is expected that this section of Trace will have the most development as more functions are requested from acoustic engineers and implemented as needs arise.

## Air Absorption

`Private Function AirAbsorb(freq As String, Distance As Integer, temp As Integer)`

Air absorption varies over frequency with the following values (from 63Hz octave band)

|  - | 63Hz | 125Hz | 250Hz | 500Hz | 1kHz | 2kHz | 4kHz | 8kHz |
| --- | - | - | - | - | - | - | - | - |
| Insertion loss per km, dB |0.1 | 0.3 | 1.1 | 2.8 | 5 | 9 | 22.9 | 76.6 |  

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

Sound attenuation and insertion losses of sheet metal ducts are outlined in 2015 ASHRAE Handbook – HVAC Applications (SI) – Chapter 48 Sound & Vibration Control. The handbook provides the insertion loss values of a given duct size at each frequency is given by:
—	Dimensions of duct – width and height (in mm)
—	Length of duct (in m)

### Flex Duct
### End Reflection Loss (ERL)

`Function GetERL(TerminationType As String, freq As String, DuctArea As Double)`

End Reflection Loss (ERL) happens as sound waves in the low end of the frequency spectrum exit a duct into a large room, some of that sound energy gets reflected back into the duct.

The formula for End Reflection Loss is given in the ASHRAE handbook, Chapter 48 _”Noise and Vibration Control”_:

`GetERL = -10*log10(1 + ((A1 + c0) / (f * dia * Pi)) ^ A2)`

There are two types of duct terminations defined in the handbook:

- Flush
- Free Space

The duct termination changes the coefficients A1 and A2, as follows:

|   | Flush | Free |
|---|---|---|
|A1 | 0.7 | 1 |
|A2 | 2 | 2 |

Note that the variable `freq` is converted to a value using the Trace function `freqStr2Num(freq)`





### Regenerated noise
### Elbow / Bend
### Duct Split
### Silencer

Silencers are special ducts with perforated sound absorptive linings, containing elements to channel the air. These elements are known as 'splitters' or 'pods'. Silencers are designed to reduce noise travelling down ducts, but may also be used to reduce sound transmitted between spaces, also known as *crosstalk*.

![RectSilencer.png](https://github.com/Moosevellous/Trace/blob/master/img/RectSilencer.png)

The splitters or pods reduce the cross-sectional area (open area) of the duct, thereby inducing a static pressure on the system. Silencers typically come in both rectangular and circular cross sections, varying lengths, and varying open face areas. A smaller open area results in a larger static pressure on the system, and vice versa.

![SilencerCrossSection.png](https://github.com/Moosevellous/Trace/blob/master/img/SilencerCrossSection.png)

The `Silencer` function in **Trace** can either provide the insertion loss for a silencer given a known model code, or can test all silencer models to find those which meet a given noise target. All silencer data is taken from the Fantech silencer database. This data is stored in

- ...*Trace_directory*\Silencers.txt

This file contains a list of octave band insertion losses for a number of silencers, as well as the model number, length, and free area (percentage).
Refer to [https://www.fantech.com.au/attenuator.aspx](https://www.fantech.com.au/attenuator.aspx) for the default data set. Additional rows of data may be added to the text file from any source, provided it follows the same layout.

#### Search by model number

In order to search for a known silencer model:
- Type the part of the model number which is  known (RS, RT, 15C etc...)
- Hit enter or click the 'Search' button. The list will then be searched for matching text strings.
- Select a model within the search results. The insertion loss, free area %, and the length are shown.
- Click 'Insert' to add the silencer to the spreadsheet.

#### Solver

The solver function takes the following inputs:

`Function SolveForSilencer(SilRng As String, targetRng As String, NRGoal As Boolean, NoiseGoal As Double)`

In order to use the ‘find suitable silencer’ function:

- Define the 'silencer range' as any cell in the calculation row where the silencer is to be inserted.
- Set the 'target range' to any cell in the row where noise criteria has been calculated (usually the final line in the calculation).
- Select either the 'Overall dBA' or 'NR' to define the Noise Goal.
- Click 'Search'. The function will then search for a suitable silencer by testing the list of options against the Noise Goal.
- Select a model within the search results. The insertion loss, free area %, and the length are shown.
- Click 'Insert' to add the silencer to the spreadsheet.

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