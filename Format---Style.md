# Style

Styles are currently implemented for the following types:
* Title
* Unmitigated
* Mitigated
* Source
* Reference
* Silencer
* User Input
* Comment
* Subtotal
* Total
* Normal

Styles can be selected through the Trace Ribbon (applies to whole row) or through the regular Excel interface (applies to selected cells). The function [Clear](https://github.com/Moosevellous/Trace/wiki/Row-Functions#clear) applies the Trace Normal style.

# Units

Formatting to cells is applied through the NumberFormat attribute in VBA. Formatting of units can be applied with this button for the following unit types:

* metres (m)
* metres squared (m<sup>2</sup>)
* metres squared per second (m<sup>2</sup>/s)
* metres cubed per second (m<sup>2</sup>/s)
* decibels (dB)
* A-weighted decibels (dBA)
* kilowatts (kW)
* Pascals (Pa)
* Directivity (Q)

# Target

Applies colouring to the cells using a 'traffic light' system. Conditional formatting will be applied to the correct columns to indicate when the target it met, marginal or exceeds.