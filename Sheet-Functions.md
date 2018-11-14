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

Generates a plot of the rows selected and applied the following formatting:
- Titles the chart (using prompt)
- Titles the x and y axes
- Formats the y-axis to be in whole numbers
- Separates overall A-weighted value so it appears as a single point
- Scales the graph to have good proportions

# Heat Map
Uses conditional formatting to generate a heat map of values. 
- Green = Less
- Red = More

# Fix References
Excel formulas sometimes point to places they shouldn't. In case they do this, select a cell with the formula error and click the Fix References button. The part of the formula bounded by apostrophes and exclamation marks (this is the reference bit) is replaced with an empty string (""). This usually fixes things.