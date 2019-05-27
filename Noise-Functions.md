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

When the source is sufficiently small, we assume that the origin  of the sound is a single point in space that spreads out spherically, such that the sound pressure per area is reduced by the area of a sphere, `4*PI(R^2)`. The Q term to be either 1, 2, 4, or 8 to represent a fraction of a sphere, either pure spherical spreading (Q = 1), half spherical spreading (Q=2) and so on, up to a source in the corner of a room, 1/8th spherical spreading (Q = 8).

![QFactorDiagram.gif](https://github.com/Moosevellous/Trace/blob/master/img/QFactorDiagram.gif)

We can then convert this reduction in sound pressure level by taking 10*log of the previous equation.

The formula is therefore: `10*log(Q/4*PI(R^2))`

As one would expect, a doubling of the Q factor  results in a 3dB  reduction in distance attenuation.

### Line Source
Consider a line source as spreading out as the surface area of a cylinder 2*PI(R^2). The Q term to be either 1, 2, 4, or 8 to represent a fraction of a cylinder, either pure cylindrical spreading (Q = 1), half cylindrical spreading (Q=2) and so on.

The formula is therefore: `10*log(Q/2*PI*R)`

![QFactorDiagramCyl.gif](https://github.com/Moosevellous/Trace/blob/master/img/QFactorDiagramCyl.gif)

### Plane Source
`-10*LOG(H*L)+10*LOG(ATAN((H*L)/(L*R*SQRT((H^2)+(L^2)+(4*R^2)))))-2`

For a plane source, the attenuation depends on 3 variables. The height of the plane, the width of the plane, and the distance from the plane (H, L, and R respectively).

It should be noted that while the above equation looks intimidating, at large distances, the equation converges to spreading as a point source. The criteria for this is approximately for distances of R = SQRT(H*L). This is because as you move further away from a plane source, eventually, the distance term R becomes vastly more important in the spreading that the size of the plane itself.

![DistanceAttenuation](https://github.com/Moosevellous/Trace/blob/master/img/distanceAtten.png)

### Area correction
The correction for area is:
`10*log(A)`

The button `Area Correction` applies the formula to all octave bands or one-third octave bands.

## Mech Elements
### ASHRAE Duct

`Function GetASHRAE(freq As String, W As Double, H As Double, XXX As String, L As String)`

Sound attenuation and insertion losses of sheet metal ducts are outlined in ASHRAE handbook, Chapter 48 _”Noise and Vibration Control”_. The handbook provides the insertion loss values of a given duct size at each frequency is given by:

- Dimensions of duct – width and height (in mm)
- Length of duct (in m)

The insertion loss values are provided in the following tables:

1. Unlined rectangular sheet metal ducts

| Duct size, mm | 63Hz | 125Hz | 250Hz | >250Hz |
| - | - | - | - | - |
| 150 x 150 | 0.98 | 0.66 | 0.33 | 0.33 |
| 305 x 305 | 1.15 | 0.66 | 0.33 | 0.20 |	
| 305 x 610 | 1.31 | 0.66 | 0.33 | 0.16 |		
| 610 x 610 | 0.82 | 0.66 | 0.33 | 0.10	|
| 1220 x 1220 | 0.49 | 0.33 | 0.23 | 0.07 |	
| 1830 x 1830 | 0.33 | 0.33 | 0.16 | 0.07 |	

2. Rectangular sheet metal ducts with 25mm fibreglass lining

| Duct size, mm	|  	| 125Hz	| 250Hz	| 500Hz	| 1000Hz | 2000Hz | 4000Hz| 
|-------	|------	|------	|-------	|------	|------	|-------	|------	|------	|
| 150 	| 150 	| 2.0 	| 4.9 	| 8.9 	| 19.0 	| 24.3 	| 14.1 	|  	|
| 150 	| 250 	| 1.6 	| 3.9 	| 7.9 	| 16.7 	| 20.0 	| 12.1 	|  	|
| 200 	| 200 	| 1.6 	| 3.9 	| 7.5 	| 16.4 	| 19.0 	| 11.8 	|  	|
| 150 	| 300 	| 1.6 	| 3.9 	| 7.5 	| 16.4 	| 19.0 	| 11.8 	|  	|
| 200 	| 300 	| 1.3 	| 3.3 	| 6.9 	| 14.8 	| 16.1 	| 10.5 	|  	|
| 250 	| 250 	| 1.3 	| 3.3 	| 6.9 	| 14.4 	| 15.4 	| 10.2 	|  	|
| 150 	| 460 	| 1.6 	| 3.3 	| 7.2 	| 15.4 	| 17.1 	| 10.8 	|  	|
| 300 	| 300 	| 1.3 	| 2.6 	| 6.2 	| 13.1 	| 13.5 	| 9.2 	|  	|
| 200 	| 460 	| 1.3 	| 3.0 	| 6.6 	| 14.1 	| 14.8 	| 9.8 	|  	|
| 250 	| 410 	| 1.3 	| 2.6 	| 6.2 	| 13.1 	| 13.1 	| 8.9 	|  	|
| 200 	| 610 	| 1.3 	| 2.6 	| 6.2 	| 13.1 	| 13.5 	| 9.2 	|  	|
| 250 	| 510 	| 1.0 	| 2.6 	| 5.9 	| 12.5 	| 12.1 	| 8.5 	|  	|
| 300 	| 460 	| 1.0 	| 2.3 	| 5.6 	| 12.1 	| 11.5 	| 8.2 	|  	|
| 380 	| 380 	| 1.0 	| 2.3 	| 5.6 	| 11.8 	| 10.8 	| 7.9 	|  	|
| 300 	| 610 	| 1.0 	| 2.0 	| 5.6 	| 11.5 	| 10.5 	| 7.5 	|  	|
| 250 	| 760 	| 1.0 	| 2.3 	| 5.6 	| 11.8 	| 10.8 	| 7.9 	|  	|
| 460 	| 460 	| 1.0 	| 2.0 	| 5.2 	| 10.8 	| 9.5 	| 7.2 	|  	|
| 380 	| 560 	| 1.0 	| 2.0 	| 5.2 	| 10.8 	| 9.5 	| 7.2 	|  	|
| 300 	| 910 	| 1.0 	| 2.0 	| 5.2 	| 10.8 	| 9.5 	| 7.2 	|  	|
| 380 	| 760 	| 1.0 	| 1.6 	| 4.9 	| 10.2 	| 8.5 	| 6.6 	|  	|
| 460 	| 710 	| 0.7 	| 1.6 	| 4.6 	| 9.8 	| 7.9 	| 6.2 	|  	|
| 610 	| 610 	| 0.7 	| 1.6 	| 4.6 	| 9.2 	| 7.2 	| 5.9 	|  	|
| 460 	| 910 	| 0.7 	| 1.6 	| 4.6 	| 9.2 	| 7.2 	| 5.9 	|  	|
| 380 	| 1140 	| 0.7 	| 1.6 	| 4.6 	| 9.5 	| 7.9 	| 6.2 	|  	|
| 610 	| 910 	| 0.7 	| 1.3 	| 3.9 	| 8.5 	| 6.2 	| 5.2 	|  	|
| 760 	| 760 	| 0.7 	| 1.3 	| 3.9 	| 8.2 	| 5.9 	| 5.2 	|  	|
| 460 	| 1370 	| 0.7 	| 1.3 	| 4.3 	| 8.9 	| 6.6 	| 5.6 	|  	|
| 610 	| 1220 	| 0.7 	| 1.3 	| 3.9 	| 7.9 	| 5.6 	| 4.9 	|  	|
| 910 	| 910 	| 0.7 	| 1.0 	| 3.6 	| 7.5 	| 5.2 	| 4.6 	|  	|
| 760 	| 1140 	| 0.7 	| 1.0 	| 3.6 	| 7.5 	| 5.2 	| 4.6 	|  	|
| 610 	| 1830 	| 0.7 	| 1.0 	| 3.6 	| 7.5 	| 5.2 	| 4.6 	|  	|
| 1070 	| 1070 	| 0.7 	| 1.0 	| 3.3 	| 6.9 	| 4.6 	| 4.3 	|  	|
| 760 	| 1520 	| 0.7 	| 1.0 	| 3.6 	| 7.2 	| 4.6 	| 4.3 	|  	|
| 910 	| 1370 	| 0.3 	| 1.0 	| 3.3 	| 6.9 	| 4.3 	| 3.9 	|  	|
| 1220 	| 1220 	| 0.3 	| 1.0 	| 3.3 	| 6.6 	| 3.9 	| 3.9 	|  	|
| 910 	| 1830 	| 0.3 	| 1.0 	| 3.3 	| 6.6 	| 3.9 	| 3.9 	|  	|
| 910 	| 1830 	| 0.3 	| 0.7 	| 3.0 	| 6.2 	| 3.6 	| 3.6 	|  	|
| 760 	| 2290 	| 0.3 	| 1.0 	| 3.3 	| 6.9 	| 4.3 	| 3.9 	|  	|
| 1070 	| 1630 	| 0.3 	| 1.0 	| 3.0 	| 6.2 	| 3.9 	| 3.6 	|  	|
| 1220 	| 1830 	| 0.3 	| 0.7 	| 3.0 	| 5.9 	| 3.3 	| 3.3 	|  	|
| 1070 	| 2130 	| 0.3 	| 0.7 	| 3.0 	| 5.9 	| 3.6 	| 3.6 	|  	|
| 1020 	| 2440 	| 0.3 	| 0.7 	| 2.6 	| 5.6 	| 3.3 	| 3.3 	|  	|
| 1070 	| 3200 	| 0.3 	| 0.7 	| 3.0 	| 5.6 	| 3.3 	| 3.3 	|  	|
| 1020 	| 3660 	| 0.3 	| 0.7 	| 2.6 	| 5.2 	| 3.0 	| 3.0 	|  	|


3. Rectangular sheet metal ducts with 50mm fibreglass lining
| *50 R 	|  	| 125 	| 250 	| 500 	| 1000 	| 2000 	| 4000 	|  	|  	|
| 150 	| 150 	| 2.6 	| 9.5 	| 16.1 	| 23.6 	| 24.3 	| 14.1 	|  	|  	|
| 150 	| 250 	| 2.3 	| 7.9 	| 14.4 	| 21.0 	| 20.0 	| 12.1 	|  	|  	|
| 200 	| 200 	| 2.0 	| 7.5 	| 13.8 	| 20.3 	| 19.0 	| 11.8 	|  	|  	|
| 150 	| 300 	| 2.0 	| 7.5 	| 13.8 	| 20.3 	| 19.0 	| 11.8 	|  	|  	|
| 200 	| 300 	| 2.0 	| 6.2 	| 12.8 	| 18.4 	| 136.1 	| 10.5 	|  	|  	|
| 250 	| 250 	| 2.0 	| 6.2 	| 12.5 	| 18.0 	| 15.4 	| 10.2 	|  	|  	|
| 150 	| 460 	| 2.0 	| 6.9 	| 13.1 	| 19.0 	| 17.1 	| 10.8 	|  	|  	|
| 300 	| 300 	| 1.6 	| 5.2 	| 11.5 	| 16.4 	| 13.5 	| 9.2 	|  	|  	|
| 200 	| 460 	| 1.6 	| 5.9 	| 12.1 	| 17.7 	| 14.8 	| 9.8 	|  	|  	|
| 250 	| 410 	| 1.6 	| 5.2 	| 11.2 	| 16.4 	| 13.1 	| 8.9 	|  	|  	|
| 200 	| 610 	| 1.6 	| 5.2 	| 11.5 	| 16.4 	| 13.5 	| 9.2 	|  	|  	|
| 250 	| 510 	| 1.3 	| 4.9 	| 10.8 	| 15.7 	| 12.1 	| 8.5 	|  	|  	|
| 300 	| 460 	| 1.3 	| 4.6 	| 10.5 	| 15.1 	| 11.5 	| 8.2 	|  	|  	|
| 380 	| 380 	| 1.3 	| 4.3 	| 10.2 	| 14.8 	| 10.8 	| 7.9 	|  	|  	|
| 300 	| 610 	| 1.3 	| 4.3 	| 9.8 	| 14.1 	| 10.5 	| 7.5 	|  	|  	|
| 250 	| 760 	| 1.3 	| 4.3 	| 10.2 	| 14.8 	| 10.8 	| 7.9 	|  	|  	|
| 460 	| 460 	| 1.3 	| 3.9 	| 9.5 	| 13.5 	| 9.5 	| 7.2 	|  	|  	|
| 380 	| 560 	| 1.3 	| 3.9 	| 9.5 	| 13.5 	| 9.5 	| 7.2 	|  	|  	|
| 300 	| 910 	| 1.3 	| 3.9 	| 9.5 	| 13.5 	| 9.5 	| 7.2 	|  	|  	|
| 380 	| 760 	| 1.0 	| 3.6 	| 8.9 	| 12.8 	| 8.5 	| 6.6 	|  	|  	|
| 460 	| 710 	| 1.0 	| 3.3 	| 8.5 	| 12.1 	| 7.9 	| 6.2 	|  	|  	|
| 610 	| 610 	| 1.0 	| 3.0 	| 8.2 	| 11.5 	| 7.2 	| 5.9 	|  	|  	|
| 460 	| 910 	| 1.0 	| 3.0 	| 8.2 	| 11.5 	| 7.2 	| 5.9 	|  	|  	|
| 380 	| 1140 	| 1.0 	| 3.3 	| 8.5 	| 11.8 	| 7.9 	| 6.2 	|  	|  	|
| 610 	| 910 	| 1.0 	| 2.6 	| 7.5 	| 10.5 	| 6.2 	| 5.2 	|  	|  	|
| 760 	| 760 	| 0.7 	| 2.6 	| 7.2 	| 10.2 	| 5.9 	| 5.2 	|  	|  	|
| 460 	| 1370 	| 1.0 	| 2.6 	| 7.5 	| 10.8 	| 6.6 	| 5.6 	|  	|  	|
| 610 	| 1220 	| 0.7 	| 2.3 	| 7.2 	| 9.8 	| 5.6 	| 4.9 	|  	|  	|
| 910 	| 910 	| 0.7 	| 2.3 	| 6.6 	| 9.5 	| 5.2 	| 4.6 	|  	|  	|
| 760 	| 1140 	| 0.7 	| 2.3 	| 6.6 	| 9.5 	| 5.2 	| 4.6 	|  	|  	|
| 610 	| 1830 	| 0.7 	| 2.3 	| 6.6 	| 9.5 	| 5.2 	| 4.6 	|  	|  	|
| 1070 	| 1070 	| 0.7 	| 2.0 	| 6.2 	| 8.5 	| 4.6 	| 4.3 	|  	|  	|
| 760 	| 1520 	| 0.7 	| 2.0 	| 6.2 	| 8.9 	| 4.6 	| 4.3 	|  	|  	|
| 910 	| 1370 	| 0.7 	| 2.0 	| 6.2 	| 8.5 	| 4.3 	| 3.9 	|  	|  	|
| 1220 	| 1220 	| 0.7 	| 1.6 	| 5.9 	| 8.2 	| 3.9 	| 3.9 	|  	|  	|
| 910 	| 1830 	| 0.7 	| 1.6 	| 5.9 	| 8.2 	| 3.9 	| 3.9 	|  	|  	|
| 910 	| 1830 	| 0.7 	| 1.6 	| 5.6 	| 7.5 	| 3.6 	| 3.6 	|  	|  	|
| 760 	| 2290 	| 0.7 	| 1.6 	| 5.9 	| 8.5 	| 4.3 	| 3.9 	|  	|  	|
| 1070 	| 1630 	| 0.7 	| 1.6 	| 5.6 	| 7.9 	| 3.9 	| 3.6 	|  	|  	|
| 1220 	| 1830 	| 0.7 	| 1.3 	| 5.2 	| 7.5 	| 3.3 	| 3.3 	|  	|  	|
| 1070 	| 2130 	| 0.7 	| 1.6 	| 5.2 	| 7.5 	| 3.6 	| 3.6 	|  	|  	|
| 1020 	| 2440 	| 0.3 	| 1.3 	| 4.9 	| 6.9 	| 3.3 	| 3.3 	|  	|  	|
| 1070 	| 3200 	| 0.3 	| 1.3 	| 5.2 	| 7.2 	| 3.3 	| 3.3 	|  	|  	|
| 1020 	| 3660 	| 0.1 	| 1.3 	| 4.9 	| 6.6 	| 3.0 	| 3.0 	|  	|  	|
| *25 C 	| 63 	| 125 	| 250 	| 500 	| 1000 	| 2000 	| 4000 	| 8000 	|  	|
| 150 	| 1.25 	| 1.94 	| 3.05 	| 5.02 	| 7.12 	| 7.58 	| 6.69 	| 4.13 	|  	|
| 205 	| 1.05 	| 1.77 	| 2.92 	| 4.92 	| 7.19 	| 7.12 	| 6.00 	| 3.87 	|  	|
| 255 	| 0.89 	| 1.64 	| 2.79 	| 4.86 	| 7.22 	| 6.69 	| 5.38 	| 3.67 	|  	|
| 305 	| 0.75 	| 1.51 	| 2.66 	| 4.76 	| 7.15 	| 6.27 	| 4.86 	| 3.44 	|  	|
| 355 	| 0.62 	| 1.38 	| 2.53 	| 4.69 	| 7.02 	| 5.87 	| 4.40 	| 3.28 	|  	|
| 405 	| 0.52 	| 1.25 	| 2.4 	| 4.59 	| 6.82 	| 5.48 	| 3.97 	| 3.12 	|  	|
| 460 	| 0.43 	| 1.15 	| 2.26 	| 4.49 	| 6.59 	| 5.12 	| 3.61 	| 2.95 	|  	|
| 510 	| 0.36 	| 1.02 	| 2.13 	| 4.4 	| 6.30 	| 4.00 	| 3.28 	| 2.85 	|  	|
| 560 	| 0.26 	| 0.92 	| 2 	| 4.3 	| 5.97 	| 4.40 	| 3.02 	| 2.72 	|  	|
| 610 	| 0.23 	| 0.82 	| 1.87 	| 4.2 	| 5.61 	| 4.07 	| 2.79 	| 2.62 	|  	|
| 660 	| 0.16 	| 0.72 	| 1.74 	| 4.07 	| 5.22 	| 3.74 	| 2.59 	| 2.53 	|  	|
| 710 	| 0.1 	| 0.62 	| 1.61 	| 3.94 	| 4.79 	| 3.41 	| 2.43 	| 2.43 	|  	|
| 760 	| 0.07 	| 0.52 	| 1.47 	| 3.81 	| 4.36 	| 3.12 	| 2.26 	| 2.33 	|  	|
| 815 	| 0.03 	| 0.46 	| 1.338 	| 3.67 	| 3.94 	| 2.85 	| 2.17 	| 2.26 	|  	|
| 865 	| 0 	| 0.36 	| 1.25 	| 3.51 	| 3.51 	| 2.59 	| 2.07 	| 2.17 	|  	|
| 915 	| 0 	| 0.26 	| 1.15 	| 3.35 	| 3.05 	| 2.33 	| 1.97 	| 2.10 	|  	|
| 965 	| 0 	| 0.2 	| 1.02 	| 3.15 	| 2.62 	| 2.10 	| 1.90 	| 2.00 	|  	|
| 1015 	| 0 	| 0.1 	| 0.92 	| 2.99 	| 2.23 	| 1.87 	| 1.80 	| 1.90 	|  	|
| 1070 	| 0 	| 0.03 	| 0.82 	| 2.76 	| 1.84 	| 1.64 	| 1.74 	| 1.80 	|  	|
| 1120 	| 0 	| 0 	| 0.75 	| 2.56 	| 1.48 	| 1.44 	| 1.67 	| 1.71 	|  	|
| 1170 	| 0 	| 0 	| 0.66 	| 2.33 	| 1.15 	| 1.28 	| 1.57 	| 1.57 	|  	|
| 1220 	| 0 	| 0 	| 0.59 	| 2.07 	| 0.85 	| 1.12 	| 1.48 	| 1.44 	|  	|
| 1270 	| 0 	| 0 	| 0.49 	| 1.8 	| 0.62 	| 0.95 	| 1.35 	| 1.31 	|  	|
| 1320 	| 0 	| 0 	| 0.46 	| 1.51 	| 0.43 	| 0.82 	| 1.21 	| 1.12 	|  	|
| 1370 	| 0 	| 0 	| 0.39 	| 1.21 	| 0.30 	| 0.72 	| 1.02 	| 0.95 	|  	|
| 1420 	| 0 	| 0 	| 0.33 	| 0.9 	| 0.26 	| 0.59 	| 0.82 	| 0.72 	|  	|
| 1475 	| 0 	| 0 	| 0.3 	| 0.56 	| 0.26 	| 0.42 	| 0.59 	| 0.49 	|  	|
| 1525 	| 0 	| 0 	| 0.26 	| 0.52 	| 0.33 	| 0.46 	| 0.30 	| 0.23 	|  	|
| *50 C 	| 63 	| 125 	| 250 	| 500 	| 1000 	| 2000 	| 4000 	| 8000 	|  	|
| 150 	| 1.84 	| 2.62 	| 4.49 	| 7.38 	| 7.12 	| 7.58 	| 6.69 	| 4.13 	|  	|
| 205 	| 1.67 	| 2.46 	| 4.36 	| 7.32 	| 7.19 	| 7.12 	| 6.00 	| 3.87 	|  	|
| 255 	| 1.51 	| 2.33 	| 4.23 	| 7.22 	| 7.22 	| 6.69 	| 5.38 	| 3.67 	|  	|
| 305 	| 1.38 	| 2.2 	| 4.1 	| 7.15 	| 7.15 	| 6.27 	| 4.86 	| 3.44 	|  	|
| 355 	| 1.25 	| 2.07 	| 3.9 	| 7.05 	| 7.02 	| 5.87 	| 4.40 	| 3.28 	|  	|
| 405 	| 1.15 	| 1.94 	| 3.84 	| 6.96 	| 6.82 	| 5.48 	| 3.97 	| 3.12 	|  	|
| 460 	| 1.05 	| 1.84 	| 3.71 	| 6.89 	| 6.59 	| 5.12 	| 3.61 	| 2.95 	|  	|
| 510 	| 0.95 	| 1.71 	| 3.58 	| 6.79 	| 6.30 	| 4.76 	| 3.28 	| 2.85 	|  	|
| 560 	| 0.89 	| 1.61 	| 3.44 	| 6.66 	| 5.97 	| 4.40 	| 3.02 	| 2.72 	|  	|
| 610 	| 0.82 	| 1.51 	| 3.31 	| 6.56 	| 5.61 	| 4.07 	| 2.79 	| 2.62 	|  	|
| 660 	| 0.79 	| 1.41 	| 3.18 	| 6.43 	| 5.22 	| 3.74 	| 2.59 	| 2.53 	|  	|
| 710 	| 0.72 	| 1.31 	| 3.05 	| 6.33 	| 4.79 	| 3.41 	| 2.43 	| 2.43 	|  	|
| 760 	| 0.69 	| 1.21 	| 2.95 	| 6.17 	| 4.36 	| 3.12 	| 2.26 	| 2.33 	|  	|
| 815 	| 0.66 	| 1.12 	| 2.82 	| 6.04 	| 3.94 	| 2.85 	| 2.17 	| 2.26 	|  	|
| 865 	| 0.62 	| 1.05 	| 2.69 	| 5.87 	| 3.51 	| 2.59 	| 2.07 	| 2.17 	|  	|
| 915 	| 0.59 	| 0.95 	| 2.59 	| 5.71 	| 3.05 	| 2.33 	| 1.97 	| 2.10 	|  	|
| 965 	| 0.56 	| 0.89 	| 2.49 	| 5.54 	| 2.62 	| 2.10 	| 1.90 	| 2.00 	|  	|
| 1015 	| 0.52 	| 0.79 	| 2.4 	| 5.35 	| 2.23 	| 1.87 	| 1.80 	| 1.90 	|  	|
| 1070 	| 0.49 	| 0.72 	| 2.3 	| 5.15 	| 1.84 	| 1.64 	| 1.74 	| 1.80 	|  	|
| 1120 	| 0.43 	| 0.66 	| 2.2 	| 4.92 	| 1.48 	| 1.44 	| 1.67 	| 1.71 	|  	|
| 1170 	| 0.39 	| 0.56 	| 2.1 	| 4.69 	| 1.15 	| 1.28 	| 1.57 	| 1.57 	|  	|
| 1220 	| 0.36 	| 0.49 	| 2.03 	| 4.46 	| 0.85 	| 1.12 	| 1.48 	| 1.44 	|  	|
| 1270 	| 0.3 	| 0.9 	| 1.97 	| 4.2 	| 0.62 	| 0.95 	| 1.35 	| 1.31 	|  	|
| 1320 	| 0.23 	| 0.33 	| 1.9 	| 3.9 	| 0.43 	| 0.82 	| 1.21 	| 1.12 	|  	|
| 1370 	| 0.16 	| 0.26 	| 1.84 	| 3.61 	| 0.30 	| 0.72 	| 1.02 	| 0.95 	|  	|
| 1420 	| 0.07 	| 0.16 	| 1.8 	| 3.28 	| 0.26 	| 0.59 	| 0.82 	| 0.72 	|  	|
| 1475 	| 0 	| 0.1 	| 1.74 	| 2.95 	| 0.26 	| 0.52 	| 0.59 	| 0.49 	|  	|
| 1525 	| 0 	| 0 	| 1.74 	| 2.59 	| 0.33 	| 0.46 	| 0.30 	| 0.23 	|  	|
| *0 R 	|  	| 63 	| 125 	| 250 	| 500 	| 1000 	| 2000 	| 4000 	|  	|
| 150 	| 150 	| 0.98 	| 0.66 	| 0.33 	| 0.33 	| 0.33 	| 0.33 	| 0.33 	|  	|
| 305 	| 305 	| 1.15 	| 0.66 	| 0.33 	| 0.20 	| 0.20 	| 0.20 	| 0.20 	|  	|
| 305 	| 610 	| 1.31 	| 0.66 	| 0.33 	| 0.16 	| 0.16 	| 0.16 	| 0.16 	|  	|
| 610 	| 610 	| 0.82 	| 0.66 	| 0.33 	| 0.10 	| 0.10 	| 0.10 	| 0.10 	|  	|
| 1220 	| 1220 	| 0.49 	| 0.33 	| 0.23 	| 0.07 	| 0.07 	| 0.07 	| 0.07 	|  	|
| 1830 	| 1830 	| 0.33 	| 0.33 	| 0.16 	| 0.07 	| 0.07 	| 0.07 	| 0.07 	|  	|
| *0 C 	| 63 	| 125 	| 250 	| 500 	| 1000 	| 2000 	| 4000 	|  	|  	|
| 180 	| 0.1 	| 0.1 	| 0.16 	| 0.16 	| 0.33 	| 0.33 	| 0.33 	|  	|  	|
| 380 	| 0.1 	| 0.1 	| 0.1 	| 0.16 	| 0.23 	| 0.23 	| 0.23 	|  	|  	|
| 760 	| 0.07 	| 0.07 	| 0.07 	| 0.1 	| 0.16 	| 0.16 	| 0.16 	|  	|  	|
| 1520 	| 0.03 	| 0.03 	| 0.03 	| 0.07 	| 0.07 	| 0.07 	| 0.07 	|  	|  	|
4. Unlined straight round ducts

| Diameter | 63 | 125 | 250 | 500 | 1000 | 2000 | 4000 |
| - | - | - | - | - | - | - | - |		
| 180 | 0.1 | 0.1 | 0.16 | 0.16 | 0.33 | 0.33 |	0.33 |		
| 380 | 0.1 | 0.1 | 0.1 | 0.16 | 0.23 | 0.23 | 0.23 |	
| 760 | 0.07 | 0.07 | 0.07 | 0.1 | 0.16 | 0.16 | 0.16|		
| 1520 | 0.03 | 0.03 | 0.03 | 0.07 | 0.07 | 0.07 | 0.07 |	

5. Acoustically lined round ducts with 25mm lining
6. Acoustically lined round ducts with 50mm lining

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

![RectSilencer.png](https://github.com/Moosevellous/Trace/blob/master/img/RectSilencer.png) ![CircSilencer.png](https://github.com/Moosevellous/Trace/blob/master/img/CircSilencer.png) 


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
Inserts the transmission loss from louvres in text file:
- ...*Trace_directory*\Louvres.txt

![frmLouvres.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmLouvres.JPG)

The values are into the sheet, including a comment containing the length and open area of the louvre.

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