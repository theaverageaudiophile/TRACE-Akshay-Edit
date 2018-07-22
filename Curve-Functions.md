# NR

The button NR curve inserts two functions. The function `NRRate` finds the corresponding NR curve for the row above. The function `NRCurve` returns the values of the curve at each octave band.

## NR Rate

`Function NR_rate(DataTable As Variant, Optional fstr As String)`

NR curves are defined in *AS1469:1983 Acoustics - Methods for the determination of noise rating numbers*. The standard includes a method for comparing a given sound spectrum to the set of curves and returning the corresponding curve.

![NR Curves.png](https://github.com/Moosevellous/Trace/blob/master/img/NR%20curves.png)

|  -  | 31.5 | 63 | 125 | 250 | 500 | 1k | 2k | 4k | 8k | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| A_f | 55.4 |  35.5 |  22 |  12 |  4.8 |  0 |  -3.5 |  -6.1 |  -8 | 
| B_f | 0.681 |  0.79 |  0.87 |  0.93 |  0.974 |  1 |  1.015 |  1.025 |  1.03 | 

The values of a given NR curve at each frequency is given by:

`NRcurve = A_f+(B_f*Curve_no)`

### NR Curve
`Function NRcurve(Curve_no As Integer, fstr As String)`

Some applications for various NR curves are:

| Rating | Application |
|---|---|
| NR 25 | Concert halls, broadcasting and recording studios, churches  |
| NR 30 | Private dwellings, hospitals, theatres, cinemas, conference rooms  |
| NR 35 | Libraries, museums, court rooms, schools, hospitals, hotels, executive offices |
| NR 40 | Halls, corridors, restaurants, night clubs, offices, shops  |
| NR 45 | Department stores, supermarkets, canteens, general offices |
| NR 50 | Typing pools, offices with business machines  |
| NR 55 | Light engineering works |
| NR 60 | Foundries, heavy engineering works |

# NC

## NC Curve

`Function NCcurve(Curve_no As Integer, fstr As String)`

![NC Curves.png](https://github.com/Moosevellous/Trace/blob/master/img/NC%20curves.png)

# R<sub>w </sub>
The button 'Rw Curve' inserts two functions. The function `RwRate` finds the corresponding Rw curve for the row above. The function `RwCurve` returns the values of the curve at each octave or one-third-octave band.

## RwCurve

`Function RwCurve(CurveNo As Variant, fstr As String) `

**Rw** is a rating curve defined in *ISO717.1 - Acoustics - Rating of sound insulation in buildings and of building elements -- Part 1: Airborne sound insulation*. The standard defines a family of curves of the same shape and the rules for rating a given set of transmission loss data against the set of curves. 

| Reference curve | 100   | 125   | 160   | 200   | 250  | 315  | 400  | 500  | 630  | 800  | 1k   | 1.25k | 1.6k | 2k   | 2.5k | 3.15k |
|-------|-------|-------|-------|-------|------|------|------|------|------|------|------|-------|------|------|------|-------|
| 1/3 octave band | 33   | 36   | 39   | 42   | 45  | 48  | 51  | **52**  | 53  | 54  | 55  | 56   | 56  | 56  | 56  | 56   |
| Octave band |       | 36   |       |       | 45  |      |      | **52**  |      |      | 55  |       |      | 56  |      |       |

The name/value of a particular curve is given by the value at the 500Hz band. 

![RwCurves.png](https://github.com/Moosevellous/Trace/blob/master/img/RwCurves.png)

## RwRate
`Function RwRate(DataTable As Variant, Optional Mode As String)`

*ISO717.1* requires that the corresponding curve have a sum of deficiencies less than:
- 32 dB (in one third octave bands)
or
- 10 dB (in octave bands)

The function adds each band that is below the curve to `SumDeficiencies`. If the result is less than the allowable sum of deficiencies, the curve is moved up by 1dB and the process loops. Once the maximum allowable sum of deficiencies is exceeded, the curve is moved down 1dB (as this sum of deficiencies is not allowed). 

## C<sub>tr</sub> Rate

`Function CtrRate(DataTable As Variant, rw As Integer)`

# L<sub>nw</sub>

The Lnw rating curve is defined in *ISO717.2 Acoustics - Rating of sound insulation in buildings and of building elements  -- Part 2: Impact sound insulation*. The standard defines a family of curves of the same shape, and the rules for rating a given set of impact sound measurements against the set of curves. 

|   | 100 | 125 | 160 | 200 | 250 | 315 | 400 | 500 | 630 | 800 | 1k | 1.25k | 1.6k | 2k | 2.5k | 3.15k |
|---|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|----|-------|------|----|------|-------|
|Lnw62   | 62  | 62  | 62  | 62  | 62  | 62  | 61  | 60  | 59  | 58  | 57 | 54    | 51   | 48 | 45   | 42    |

## Lnw Rate

*ISO717.2* requires that the corresponding curve have a sum of deficiencies less than:
- 32 dB (in one third octave bands)
or
- 10 dB (in octave bands)

The function adds each band that is below the curve to `SumDeficiencies`. If the result is less than the allowable sum of deficiencies, the curve is moved down by 1dB and the process loops. Once the maximum allowable sum of deficiencies is exceeded, the curve is moved up 1dB (as this sum of deficiencies is not allowed). 

## Lnw Curve

`Function LnwCurve(CurveNo As Variant, fstr As String) 'Optional Mode As String)`
