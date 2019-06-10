The Header Block looks like this:
![HeaderBlock.jpg](https://github.com/Moosevellous/Trace/blob/master/img/HeaderBlock.JPG)
It's on the top of every blank sheet.

# Fill in header block
Fills in Date and Engineer initials using the user name of the computer and internal PC clock.

# Clear header block
Clears the cells in the header block of their values.

# Format Borders

Makes borders of cells look consistent. The default format looks like this:

![Borders.jpg](https://github.com/Moosevellous/Trace/blob/master/img/Borders.JPG)

# Plot

Generates a plot of the rows selected and applies formatting based on the user input form.

![frmPlotTool.JPG](https://github.com/Moosevellous/Trace/blob/master/img/frmPlotTool.JPG)

Formatting is applied to all series in the chart for consistency. Edits to an individual series can still be made the regular way through Excel's interface. The plot tool can also be applied to existing chart objects, by having the chart selected first, then clicking the Plot button in the Trace Ribbon.

# Heat Map
Uses conditional formatting to generate a heat map of values. 
- Green = Less
- Red = More

# Fix References
Excel formulas sometimes point to places they shouldn't. In case they do this, select a cell with the formula error and click the Fix References button. The part of the formula bounded by apostrophes and exclamation marks (this is the reference bit) is replaced with an empty string (""). This usually fixes things.