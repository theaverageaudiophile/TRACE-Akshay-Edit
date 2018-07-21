# Sheet Type

All Blank Calculation Sheets and [Standard Calculation Sheets](https://github.com/Moosevellous/Trace/wiki/Standard-Calculations) include a **named range** called `SheetType`. This is fed through the code to inform the macros certain information about where to copy cells, extend functions, include parameter fields etc. The Blank Calculation Sheets have the following `SheetType` values:
- `OCT` For octave band calculations, linear input
- `OCTA` For octave band calculations, A-weighted input
- `TO` For one-third-octave band calculations, linear input
- `TOA` For one-third-octave band calculations, A-weighted input

# SPLSUM

This function is called from other parts of **Trace** and can be used in any cell (even non-Trace sheets), once the add-in is installed.

# SPLAV