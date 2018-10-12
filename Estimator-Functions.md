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
# Cooling Tower

| Type | Equation |  
| --- | --- |
| Propeller <75kW | 100+8log(kW) |
| Propeller >75kW | 96+10log(kW) |
| Centrifugal <60kW | 85+11log(kW)|
| Centrifugal >60kW | 93+7log(kW) |

![frmEstCoolingTower.jpg](https://github.com/Moosevellous/Trace/blob/master/img/frmEstCoolingTower.JPG)

# Electric Motor
# Compressor