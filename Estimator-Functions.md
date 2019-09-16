|Go to...| 
|:--- |
| [Fan Simple ](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#fan-simple) <br> [Pump Simple](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#pump-simple) <br> [Cooling Tower](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#cooling-tower)<br>[Electric Motor](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#electric-motor) <br> [Compressor](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#compressor) <br> [Gas Turbine](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#gas-turbines) <br> [Steam Turbine](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#steam-turbines)  |


# Fan Simple

`Function LwFanSimple(freq As String, v As Double, P As Double, FanType As String)`

![frmFanSimple.jpg](https://github.com/Moosevellous/Trace/blob/master/img/frmFanSimple.JPG)

From Biess and Hansen, the overall Sound Power of a fan can be approximated by:

Lw=10log(v)+20log(P)+40

Where v is the volumetric flowrate of the fan

and P is the static pressure of the fan

The following spectral corrections apply to different fan types:

|  -  | 63 | 125 | 250 | 500 | 1k | 2k | 4k |  
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Forward curved centrifugal | -5 |  -10 |  -15 |  -20 |  -25 |  -28 |  -31 |  
| Backward curved centrifugal | -10 |  -11 |  -10 |  -15 |  -20 |  -25 |  -30 |  
| Radial or paddle blade | 3 |  -3 |  -10 |  -11 |  -15 |  -19 |  -23 |  
| Axial | -8 |  -8 |  -6 |  -7 |  -8 |  -12 |  -16 |  
| Bifurcated| -3 |  -3 |  -4 |  -5 |  -7 |  -8 |  -11 | 
| Propeller fan (approx)  | -3 |  -4 |  -1 |  -8 |  -12 |  -13 |  -20 |  
| Variable inlet vanes - 100% | 0 |  0 |  0 |  0 |  0 |  0 |  0 |  
| Variable inlet vanes - 80% | 8 |  5 |  4 |  4 |  4 |  4 |  4 |  
| Variable inlet vanes - 60% | 8 |  7 |  6 |  5 |  5 |  5 |  5 | 
| Variable inlet vanes - 40% | 3 |  2 |  1 |  0 |  0 |  0 |  1 | 

<a href="#">[Back to top]</a>

# Pump Simple

Pumps are pieces of mechanical plant which move a fluid through a system. The motor on the pump is usually the main source of noise. 

![CentrifPump.png](https://github.com/Moosevellous/Trace/blob/master/img/CentrifPump.png)

To estimate the sound levels from pumps, the estimator tool uses the method from  _Engineering Noise Control_ (Bies and Hansen) Table 11.10: 

| Speed range(RPM) | Equation |  |
| --- | --- | --- |
|  | Under 75kW | Above 75kW |
| 3000-3600 | 72+10log(kW) | 86+3log(kW) |
| 1600-1800 | 75+10log(kW) | 89+3log(kW) |
| 1000-1500 | 70+10log(kW) | 84+3log(kW) |
| 450-900 | 68+10log(kW) | 82+3log(kW) |

The Pump estimator tool looks like this:

![frmEstPumpLw.jpg](https://github.com/Moosevellous/Trace/blob/master/img/frmEstPumpLw.JPG)

The corrections to derive sound pressure at 1 metre are as follows:

| Octave Band | 31.5 | 63 | 125 | 250 | 500 | 1k | 2k | 4k | 8k |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Correction | -13 | -12 | -11 | -9 | -9 | -6 | -9 | -13 | -19 |

<a href="#">[Back to top]</a>

# Cooling Tower

Cooling towers are pieces of mechanical plant used in many air conditioning systems to cool the water used in the compressor coils. This is achieved by blowing air through a stream of water, whereby the evaporation of some of this water will lead to cooling of the remaining water.

There are four main types of cooling tower configuration which must be considered:
- Centrifugal fan
  - Blow-through type
  - Axial flow
- Propeller-type
  - Induced draft
  - Forced draft, ‘underflow’

<img src="https://github.com/Moosevellous/Trace/blob/master/img/CoolingTowerTypes.JPG" width="600">

The noise levels can be estimated by knowing the operating power of the cooling tower, as well as whether it is a centrifugal or propeller type. Note that the subtypes above are for the consideration of blow through effects only.

| Type | Equation |  
| --- | --- |
| Propeller <75kW | 100+8log(kW) |
| Propeller >75kW | 96+10log(kW) |
| Centrifugal <60kW | 85+11log(kW)|
| Centrifugal >60kW | 93+7log(kW) |

<img src="https://github.com/Moosevellous/Trace/blob/master/img/CoolingTowerPowerPlot.png" width="800">

In order to convert this overall SPL to a set of octave band SPLs, the following conversion factors should be applied:

|    Cooling tower type    |    31.5    |    63    |    125    |    250    |    500    |    1k     |    2k     |    4k     |    8k     |
|--------------------------|------------|----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
|    Propeller Type        |    -8      |    -5    |    -5     |    -8     |    -11    |    -15    |    -18    |    -21    |    -29    |
|    Centrifugal Type      |    -6      |    -6    |    -8     |    -10    |    -11    |    -13    |    -12    |    -18    |    -25    |

Additionally, there are directional effects to be considered for the cooling towers when we measure the sound power level on a specific side. These effects should only be considered where the distance of measurement is >6m and are given in B&H Table 11.8 as:

|    Type of tower and   location of measurement    |    31.5    |    63    |    125    |    250    |    500    |    1k    |    2k    |    4k     |    8k    |
|------------------------|-----------|----------|-----------|-----------|-----------|----------|----------|-----------|----------|
|    **Centrifugal fan blow through**          |            |          |           |           |           |          |          |           |          |
|    Front                                          |    3       |    3     |    2      |    3      |    4      |    3     |    3     |    4      |    4     |
|    Side                                           |    0       |    0     |    0      |    -2     |    -3     |    -4    |    -5    |    -5     |    -5    |
|    Rear                                           |    0       |    0     |    -1     |    -2     |    -3     |    -4    |    -5    |    -6     |    -6    |
|    Top                                            |    -3      |    -3    |    -2     |    0      |    1      |    2     |    3     |    4      |    5     |
|    **Axial flow, blow through**                   |            |          |           |           |           |          |          |           |          |
|    Front                                          |    2       |    2     |    4      |    6      |    6      |    5     |    5     |    5      |    5     |
|    Side                                           |    1       |    1     |    1      |    -2     |    -5     |    -5    |    -5    |    -5     |    -4    |
|    Rear                                           |    -3      |    -3    |    -4     |    -7     |    -7     |    -7    |    -8    |    -11    |    -3    |
|    Top                                            |    -5      |    -5    |    -5     |    -5     |    -2     |    0     |    0     |    2      |    4     |
|   **Induced draft, propeller type**          |            |          |           |           |           |          |          |           |          |
|    Front                                          |    0       |    0     |    0      |    1      |    2      |    2     |    2     |    3      |    3     |
|    Side                                           |    -2      |    -2    |    -2     |    -3     |    -4     |    -4    |    -5    |    -6     |    -6    |
|    Top                                            |    3       |    3     |    3      |    3      |    2      |    2     |    2     |    1      |    1     |
|    **Underflow forced drafter propeller type**      |            |          |           |           |           |          |          |           |          |
|    Any side                                       |    -1      |    -1    |    -1     |    -2     |    -2     |    -3    |    -3    |    -4     |    -4    |
|    Top                                            |    2       |    2     |    2      |    3      |    3      |    4     |    4     |    5      |    5     |

In cases where the measurement is to be taken close to the intake or discharge (< 6m), data is available but it appears to be independent of the power of the plant and so has not been considered here.

The Cooling Tower estimator tool looks like this:

![frmEstCoolingTower.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmEstCoolingTower.JPG)

And implements all the corrections defined above. 

<a href="#">[Back to top]</a>

# Electric Motor

For small motors (<300kW), the equations are as follows:

|            | Equation (SPL @ 1m)        |
|------------|----------------------------|
| Under 40kW | Lp=17+17log(kW)+15log(RPM) |
| Over 40kW  | Lp=28+10log(kW)+15log(RPM) |

Corrections can be applied to Totally Enclosed Fan Cooled (TEFC) types and Drip Proof Motor (DRPR) types:

|      | 31.5 | 63 | 125 | 250 | 500 | 1k | 2k | 4k | 8k |
|------|------|----|-----|-----|-----|----|----|----|----|
| TEFC | 14   | 14 | 11  | 9   | 6   | 6  | 7  | 12 | 20 |
| DRPR | 9    | 9  | 7   | 7   | 6   | 9  | 12 | 18 | 27 |

![frmEstMotor.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmEstMotor.JPG)

<a href="#">[Back to top]</a>

# Compressor

For small compressors:

|             | 31.5 | 63 | 125 | 250 | 500 | 1k  | 2k  | 4k | 8k |
|-------------|------|----|-----|-----|-----|-----|-----|----|----|
| Up to 1.5kW | 90   | 89 | 89  | 88  | 91  | 94  | 94  | 92 | 89 |
| 2kW to 6 kW | 95   | 92 | 92  | 91  | 94  | 97  | 97  | 95 | 92 |
| 7kW to 75kW | 100  | 95 | 95  | 94  | 97  | 100 | 100 | 98 | 95 |

![frmEstCompressorSmall.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmEstCompressorSmall.JPG)

<a href="#">[Back to top]</a>

# Turbines

## Gas Turbines

Gas turbines have 3 primary sources of noise, the casing, inlet, and exhaust noise. Changes in SWL from these sources vary with the power of the turbine in MW. From Biess and Hansen , overall SWLs can be estimated by the following equations:

- Casing: Lw = 120 + 5log(MW)
- Inlet: Lw = 127 + 15log(MW)
- Outlet: Lw = 133 + 10log(MW)

From the single number SWL we can then determine octave bands by subtracting the following values from the equations above. 

| - |31.5	|63	|125	|250	|500	|1000	|2000	|4000	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Casing	|10	|7	|5	|4	|4	|4	|4	|4	|
|Inlet	|19	|18	|17	|17	|14	|8	|3	|3	|
|Outlet	|12	|8	|6	|6	|7	|9	|11	|15	|


The dialogue box which accepts the inputs of turbine power, enclosure type, and which sound source (casing, inlet or exhaust) is shown below. The tool also incorporates reductions for [enclosures](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#enclosures).


![frmEstGasTurbine.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmEstGasTurbine.JPG)

The tool allows a user to make adjustments to the desired turbine inputs and displays a live sound power spectrum which will be inserted once ok is clicked.

<a href="#">[Back to top]</a>

## Steam Turbines

Steam turbine noise emissions may be estimated in direct relation to their power, however, these turbines are typically much smaller than gas turbines and their power measured in kW rather than MW. From Biess and Hansen , an estimation of the SWL radiated is:

- Lw = 93 + 4log(kW)

From the single number SWL we can then determine octave bands by subtracting the following values from the above.

|	|31.5	|63	|125	|250	|500	|1000	|2000	|4000	|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|Casing	|11	|7	|6	|9	|10	|10	|12	|13	|


![frmEstSteamTurbine.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmEstSteamTurbine.JPG)

The tool allows a user to make adjustments to the desired turbine inputs and displays a live sound power spectrum which will be inserted once ok is clicked.The tool also incorporates reductions for [enclosures](https://github.com/Moosevellous/Trace/wiki/Estimator-Functions#enclosures).

<a href="#">[Back to top]</a>

## Enclosures

It is often the case that these turbines will be externally wrapped with insulation, in such cases, additional noise reductions should be taken in accordance with the table below.

|  |Type 1a	|Type 2b	|Type 3c	|Type 4d	|Type 5e	|
|---	|---	|---	|---	|---	|---	|
|31.5	|2	|4	|1	|3	|6	|
|63	|2	|5	|1	|4	|7	|
|125	|2	|5	|1	|4	|8	|
|250	|3	|6	|2	|5	|9	|
|500	|3	|6	|2	|6	|10	|
|1000	|3	|7	|2	|7	|11	|
|2000	|4	|8	|2	|8	|12	|
|4000	|5	|9	|3	|8	|13	|
|8000	|6	|10	|3	|8	|14	|

Type a) Glass fibre or mineral wool thermal insulation with lightweight foil over the insulation

Type b) Glass fibre or mineral wool thermal insulation with 20 or 24 gauge aluminium

Type c) Enclosing metal cabinet for the entire packaged assembly, with open ventilation holes and with no acoustic absorptive lining inside the cabinet

Type d) Enclosing metal cabinet for the entire packaged assembly, with open ventilation holes and with internal acoustic absorptive lining inside the cabinet

Type e) Enclosing metal cabinet for the entire packaged assembly with all ventilation holes into the cabinet muffled and with acoustic absorptive lining inside the cabinet


<a href="#">[Back to top]</a>