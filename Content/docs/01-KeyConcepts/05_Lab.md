# Lab 1: Key Concepts

In the labs we try to apply what we have learned. Often there is not one solution, so the approach is yours to choose. Of course, in reality there are other aspects to consider, such as performance or readability of the code. But this should not be part of the training. 

## Useful Function

***[SUM](https://dax.guide/sum/)*** <br>
Adds all the numbers in a column.

***[CALCULATE](https://dax.guide/calculate/)*** <br>
Evaluates an expression in a context modified by filters.

***[ALL](https://dax.guide/all/)*** <br>
Returns all the rows in a table, or all the values in a column, ignoring any filters that might have been applied.

***[BLANK](https://dax.guide/blank/)*** <br>
Returns a blank.

***[FORMAT](https://dax.guide/format/)*** <br>
Converts a value to text in the specified number format.

***[FILTER](https://dax.guide/filter/)*** <br>
Returns a table that has been filtered.

***[EARLIER](https://dax.guide/earlier/)*** <br>
Returns the value in the column prior to the specified number of table scans (default is 1).

***[VALUES](https://dax.guide/values/)*** <br>
When a column name is given, returns a single column table of unique values. When a table name is given, returns a table with the same columns and all the rows of the table (including duplicates) with the additional blank row if present.

***[SUMMARIZE](https://dax.guide/summarize/)*** <br>
Creates a summary of the input table grouped by the specified columns.

***[CONCATENATEX](https://dax.guide/concatenatex/)*** <br>
Evaluates expression for each row on the table, then return the concatenation of those values in a single string result, seperated by the specified delimiter.

***[CONTAINS](https://dax.guide/contains/)*** <br>
Returns TRUE if there exists at least one row where all columns have specified values.

***[IF](https://dax.guide/if/)*** <br>
Checks whether a condition is met, and returns one value if TRUE, and another value if FALSE.

***[LEFT](https://dax.guide/left/)*** <br>
Returns the specified number of characters from the start of a text string.

## Exercise

1. Calculate the profit as a Measure (SalesAmount – TotalCost)
2. Calculate the profit as a calculated column (Sales – TotalCost – DiscountAmount)
3. Create a calculated column in the Calender with the MonthNameShort (Jun, Aug etc.)
4. Create a Hierarchy in the Calendar YYYYMMDD and set the right sort order per month
5. Create a Measure that is only returning the Profit for ProductCategegory Audio
6. Create the same Measure as in 3 but avoid returning a value if no profit for a ProductCategory exists
7. Show the ratio of profit per product categegory to total profit
8. Create a calculated column that is showing accumulated profit per Day (All profit <= Date)
9. Create a measure that is displaying the selected time range (if one selected Jun 2021 if two ore more Jun 2021 – Sep 2021)
