Trace is built to be expanded by anyone. If you want to do this, it's probably a good idea to have a look at the various modules in the code and make sure you're putting it all in the right place.

All buttons are in groups on the Ribbon. The name of group should match with the name of the module in which it sits. At the moment the groups are:
- Load
- Import From
- Row Functions
- Noise Functions
- Estimator Functions
- Curve Functions
- Sheet Functions
- Help

Additionally, there is a module called `Ribbon Functions` which holds all of the callbacks for the buttons on the ribbon.

Here's how all the modules should connect:

![TraceProgramStructure.png](https://github.com/Moosevellous/Trace/blob/master/img/TraceProgramStructure.png)

In general, the sequence of events from a user clicking a button on the ribbon is as follows:
1. User clicks button on Ribbon – linked macro (`Sub btnCustomFunction()`) in `RibbonControl` module begins.
2. Macro in `RibbonControl` passes the named range *SheetType* to `Sub CustomFunction()`.
3. Sub checks for valid row range using `CheckRow()`.

***
**OPTIONAL FORM STAGE (steps 4 to 6)**

4. Sub `CustomFunction()` calls the custom form `frmFunctionName`.
5. User inputs into fields in custom form `frmFunctionName`.
6. Code within custom form places data in public variables for use by `Sub CustomFunction()`.

***

7. Sub `CustomFunction()` writes formula description.
8. Sub `CustomFunction()` merges or unmerged parameter column and applies validation.
9. Sub `CustomFunction()` writes formula in leftmost cell of template; including call of GetCustomFunction().
10. Sub `CustomFunction()` extends formula to range using function `ExtendFunction(SheetType)`.
