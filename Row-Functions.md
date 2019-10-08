| Go to... |
|:----|
|[Clear](https://github.com/Moosevellous/Trace/wiki/Row-Functions/_edit#clear) <br> [Move up](https://github.com/Moosevellous/Trace/wiki/Row-Functions#move-up) <br> [Move down](https://github.com/Moosevellous/Trace/wiki/Row-Functions#move-down) <br> [Row Reference](https://github.com/Moosevellous/Trace/wiki/Row-Functions#row-reference) <br> [Flip Sign](https://github.com/Moosevellous/Trace/wiki/Row-Functions#flip-sign) <br> [Single Correction](https://github.com/Moosevellous/Trace/wiki/Row-Functions#single-correction) <br> [Auto Sum](https://github.com/Moosevellous/Trace/wiki/Row-Functions#auto-sum) <br> [Extend Function](https://github.com/Moosevellous/Trace/wiki/Row-Functions#extend-function) <br> [10log(n)](https://github.com/Moosevellous/Trace/wiki/Row-Functions#10logn) <br> [10log(1/t)](https://github.com/Moosevellous/Trace/wiki/Row-Functions#10log1t) <br> | 
|Convert <br> [1/3 to 1/1 Octave](https://github.com/Moosevellous/Trace/wiki/Row-Functions#13-to-11-spectrum) <br> [A-Weight Spectrum](https://github.com/Moosevellous/Trace/wiki/Row-Functions#a-weight-spectrum)|

# Clear

Clears all values, formatting, comments and validation from the row. In order these are:
- `.ClearContents`
- `.Font.ColorIndex = 0`
- `.Interior.ColorIndex = 0 `
- `.Validation.Delete 'for dropdown boxes`
- `.Merge 'parameter columns`
- `.ClearComments`
- `.FormatConditions.Delete 'removes heatmap`
- `.NumberFormat = "General"`

<a href="#">[Back to top]</a>


# Move Up

Moves row of selected cell up and applies the formatting to the old row. 

<a href="#">[Back to top]</a>


# Move Down

Moves row of selected cell down and applies the formatting to the old row. 

<a href="#">[Back to top]</a>


# Row Reference

Inserts a reference to another row of the calculation, applies the *Trace Reference* Style.

<a href="#">[Back to top]</a>


# Flip Sign

Adds a negative sign to the values selected. Note, only applies to selected cells, not entire calculation row. 

<a href="#">[Back to top]</a>


# Single Correction

- Apply a single value (positive or negative) to all bands.
- ???
- Profit

<a href="#">[Back to top]</a>


# Auto Sum

Search each row above, until there is a blank row. Sum the found range of cells, in octave or one-third-octave bands.

<a href="#">[Back to top]</a>


# Extend Function

Copies the formula in column E across to all adjacent bands. Used by other formulas but can be initiated manually.

<a href="#">[Back to top]</a>


# 10Log(n)

Number of sources correction using the formula `10*log(n)`  with `n` input as a value in the parameter column

<a href="#">[Back to top]</a>


# 10Log(1/t)

Time correction using the formula `10*log(1/t)` with `t` input as a value in the parameter column.

<a href="#">[Back to top]</a>


# Convert
## 1/3 to 1/1 Spectrum
Two modes, Average and Sum. User is required to point to a cell in a `TO` or `TOA` sheet. See [SPLSUM](https://github.com/Moosevellous/Trace/wiki/Other-features#splsum) for reference.

![One Third Octave Conversion Formula](https://github.com/Moosevellous/Trace/blob/master/img/TLformula.JPG)

![One Third Octave Conversion Formula](https://github.com/Moosevellous/Trace/blob/master/img/frmConvert.JPG)

<a href="#">[Back to top]</a>


## A-Weight Spectrum
Adds the A weighting from calculation row 0 to row above selected row.


<a href="#">[Back to top]</a>

