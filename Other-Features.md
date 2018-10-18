# Sheet Type

All Blank Calculation Sheets and [Standard Calculation Sheets](https://github.com/Moosevellous/Trace/wiki/Standard-Calculations) include a **named range** called `SheetType`. This is fed through the code to inform the macros certain information about where to copy cells, extend functions, include parameter fields etc. The Blank Calculation Sheets have the following `SheetType` values:
- `OCT` For octave band calculations, linear input
- `OCTA` For octave band calculations, A-weighted input
- `TO` For one-third-octave band calculations, linear input
- `TOA` For one-third-octave band calculations, A-weighted input

# SPLSUM

`Public Function SPLSUM(ParamArray Rng1() As Variant) As Variant`

This function is called from other parts of **Trace** and can be used in any cell (even non-Trace sheets), once the add-in is installed.

The function loops through each element of `ParamArray` and uses the worksheet function `Log10` to calculate the aaddition of each Sound Pressure Level. The main part of the calculation is:

`SPLSUM = 10 * Application.WorksheetFunction.Log10((10 ^ (SPLSUM / 10)) + (10 ^ (Rng1(i) / 10)))`

# SPLAV

`Public Function SPLAV(ParamArray Rng1() As Variant) As Variant`

Calculates the average noise level, in the same method as `SPLSUM` with the extra line:

`SPLAV = SPLAV + 10 * Application.WorksheetFunction.Log10(1 / n)`

# A-weighting
More fully explained by [Wikipedia](https://en.wikipedia.org/wiki/A-weighting), the A-weighting is an attempt to correct a linear spectrum to the frequency response of the human ear. Originally built based on the 40 phon loudness curve, the A-weighting is now used for most noise measurements. 

A possible substitute curve could be:

`=10*LOG(((35041384000000000*E$6^8)/((20.598997^2+E$6^2)^2*(107.65265^2+E$6^2)*(737.86223^2+E$6^2)*(12194.217^2+E$6^2)^2)))`