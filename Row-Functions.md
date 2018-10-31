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

# Move Up

Moves row of selected cell up and applies the formatting to the old row. 

# Move Down

Moves row of selected cell down and applies the formatting to the old row. 

# Row Reference

Inserts a reference to another row of the calculation, applies the *Trace Reference* Style

# Flip Sign

Adds a negative sign to the values selected. Note, only applies to selected cells, not entire calculation row. 

# Single Correction

- Apply a single value (positive or negative) to all bands.
- ???
- Profit

# Auto Sum

Search each row above, until there is a blank row. Sum the found range of cells, in octave or one-third-octave bands.

# Extend Function

Copies the formula in column E across to all adjacent bands. Used by other formulas but can be initiated manually.

# 10Log(n)

Number of sources correction using the formula `10*log(n)`  with `n` input as a value in the parameter column

# 10Log(1/t)

Time correction using the formula `10*log(1/t)` with `t` input as a value in the parameter column

# Convert
## 1/3 to 1/1 Spectrum
Two modes, Average and Sum. User is required to point to a cell in a `TO` or `TOA` sheet. See [SPLSUM](https://github.com/Moosevellous/Trace/wiki/Other-features#splsum) for reference.

## A-Weight Spectrum
Adds the A weighting from calculation row 0 to row above.