# Lab Advanced Concepts

Query the following table expression in DAX studio:

1. Return the entire Calendar
2. Create a table with Product Category, Product Subcategory, Sales Year, and total Sales Amount
3. Create a table that includes Year and Date from calendar for the year 2006 
4. Create a table that contains all Products with Profit for year 2005, the Average Sales Quantity per Order for June 2011 and the profit margin for alle years (Revenue - Cost) / Revenue
5. Create a custom table with RegionCountryName (United States and Germany as Values) and City (Custom cities).
6. Use the custom table from exercice 5 to filter the Geography table and return only the continents that are filtered by your Countries
    + Use a FILTER operation
    + Use a Join operation
7. Create a custom table SpecialDiscount with two Rows 
    + StartDate (DATETIME) 1/1/2013 And EndDate (DATETIME) 1/6/2013 AM And Discount (DOUBLE) 0.10
    + StartDate (DATETIME) 1/6/2013 And EndDate (DATETIME) 31/12/2013 And Discount (DOUBLE) 0.30 <br>
    <br>
	StartDate (DATETIME) 1/6/2013 And EndDate (DATETIME) 31/12/2013 And Discount (DOUBLE) 0.30
	Calculate a table that gives any order a special discount, that has not already got a discount via a promotion
