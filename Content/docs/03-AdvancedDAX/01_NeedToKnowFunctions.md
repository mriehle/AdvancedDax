# Need to know Functions

**[SELECTEDVALUE](https://dax.guide/selectedvalue/)** <br>
Returns the value when there’s only one value in the specified column, otherwise returns the alternate result.

**[SWITCH](https://dax.guide/switch/)** <br>
Returns different results depending on the value of an expression.

**[FORMAT](https://dax.guide/format/)** <br>
Converts a value to text in the specified number format.

**[CONVERT](https://dax.guide/convert/)** <br>
Convert an expression to the specified data type.

**[COUNTROWS](https://dax.guide/countrows/)** <br>
Counts the number of rows in a table.

**[ISFILTERED](https://dax.guide/isfiltered/)** <br>
Returns true when there are direct filters on the specified column.

**[ISCROSSFILTERED](https://dax.guide/iscrossfiltered/)** <br>
Returns true when the specified table or column is crossfiltered.

**[FIRSTNONBLANK](https://dax.guide/firstnonblank/)** <br>
Returns the first value in the column for which the expression has a non blank value.

**[FIRSTNONBLANKVALUE](https://dax.guide/firstnonblankvalue/)** <br>
Returns the first non blank value of the expression that evaluated for the column.

**[RANKX](https://dax.guide/rankx/)** <br>
Returns the rank of an expression evaluated in the current context in the list of values for the expression evaluated for each row in the specified table.


## Time Intelligence
Time intelligence functions support calculations to compare and aggregate data over time periods, supporting days, months, quarters, and years.
**You need a  gapless  Date-Table with a Date column in your model to make these functions work.**

**[DATESYTD](https://dax.guide/datesytd/)** <br>
Returns a set of dates in the year up to the last date visible in the filter context.

**[SAMEPERIODLASTYEAR](https://dax.guide/sameperiodlastyear/)** <br>
Returns a set of dates in the current selection from the previous year.

**[DATEADD](https://dax.guide/dateadd/)** <br>
Moves the given set of dates by a specified interval.
<br>
And many more… **[Link](https://docs.microsoft.com/en-us/dax/time-intelligence-functions-dax)**
