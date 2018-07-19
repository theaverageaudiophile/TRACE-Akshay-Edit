## NR Curve
## NC Curve
## Rw 
The button 'Rw Curve' inserts two functions. The function `RwRate` finds the corresponding Rw curve for the row above. The function `RwCurve` returns the values of the curve at each octave or one-third-octave band.

### RwCurve
`Function RwCurve(CurveNo As Variant, fstr As String) `

**Rw** is a rating curve defined in *ISO717.1 - Acoustics - Rating of sound insulation in buildings and of building elements -- Part 1: Airborne sound insulation*. The standard defines a family of curves of the same shape and the rules for rating a given set of transmission loss data against the set of curves. 

`Rw_ThOct = Array(33, 36, 39, 42, 45, 48, 51, 52, 53, 54, 55, 56, 56, 56, 56, 56)`

`Rw_Oct = Array(36, 45, 52, 55, 56)`

[[https://github.com/Moosevellous/Trace/RwCurves.png]]

### RwRate
`Function RwRate(DataTable As Variant, Optional Mode As String)`

*ISO717.1* requires that the corresponding curve have a sum of deficiencies less than:
- 32 dB (in one third octave bands)
or
- 10 dB (in octave bands)

The function adds each band that is below the curve to `SumDeficiencies`. If the result is less than the allowable sum of deficiencies, the curve is moved up by 1dB and the process loops. Once the maximum allowable sum of deficiencies is exceeded, the curve is moved down 1dB (as this sum of deficiencies is not allowed). 