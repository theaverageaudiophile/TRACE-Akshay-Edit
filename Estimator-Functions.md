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

# Compressor

For small compressors:

|             | 31.5 | 63 | 125 | 250 | 500 | 1k  | 2k  | 4k | 8k |
|-------------|------|----|-----|-----|-----|-----|-----|----|----|
| Up to 1.5kW | 90   | 89 | 89  | 88  | 91  | 94  | 94  | 92 | 89 |
| 2kW to 6 kW | 95   | 92 | 92  | 91  | 94  | 97  | 97  | 95 | 92 |
| 7kW to 75kW | 100  | 95 | 95  | 94  | 97  | 100 | 100 | 98 | 95 |

# Turbines

## Steam Turbines

## Gas Turbines