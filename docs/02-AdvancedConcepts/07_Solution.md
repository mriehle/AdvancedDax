# Lab Solution

Query the following table expression in DAX studio agains the Contos PowerBI model:

1. Return the entire Calendar
```dax
EVALUATE
'Calendar'
```
2. Create a table with Product Category, Product Subcategory, Sales Year, and total Sales Amount
```dax
EVALUATE
SUMMARIZECOLUMNS (
    ProductCategory[ProductCategory],
    ProductSubcategory[ProductSubcategory],
    'Calendar'[YearName],
    "Profit", [Profit]
)
```
3. Create a table that includes Year and Date from calendar for the year 2006 
```dax
// Option A: Use FILTER
EVALUATE
FILTER (
    SELECTCOLUMNS (
        'Calendar',
        "@Date", 'Calendar'[Date],
        "@Year", 'Calendar'[YearName]
    ),
    [@Year] = "2006"
)

// Option B: Use CALCULATETABLE
EVALUATE
CALCULATETABLE( 
    SELECTCOLUMNS (
        'Calendar',
        "@Date", 'Calendar'[Date],
        "@Year", 'Calendar'[YearName]
    ),
    'Calendar'[Year] = 2006
)
```
4. Create a table that contains all Products with Profit for year 2005, the Average Sales Quantity per Order for June 2011 and the profit margin for alle years (Sales - Cost) / Sales
```dax
EVALUATE
ADDCOLUMNS (
    VALUES ( ProductCategory ),
    "Profit2006", FORMAT ( CALCULATE ( [Profit], 'Calendar'[Year] = 2011 ), """US$"" #,0.00" ),
    "AvgSalesLast30Days",
        CALCULATE (
            AVERAGE ( Sales[SalesQuantity] ),
            'Calendar'[Year] = 2011,
            'Calendar'[MonthName] = "July"
        ),
    "ProfitMargin", FORMAT ( DIVIDE ( [Profit], SUM ( sales[SalesAmount] ) ), "Percent" ))
```
5. Create a custom table with RegionCountryName (United States and Germany as Values) and City (Custom cities).
```dax
EVALUATE
DATATABLE (
    "RegionCountryName", STRING,
    "City", STRING,
    {
        { "United States", "New York" },
        { "United States", "Los Angeles" },
        { "United States", "Miami" },
        { "United States", "Atlanta" },
        { "Germany", "Berlin"},
        { "Germany", "Frankfurt"}
    }
)
```
6. Use the custom table from exercice 5 to filter the Geography table and return only the continents that are filtered by your Countries
    + Use a FILTER operation
    + Use a Join operation
```dax
DEFINE
    VAR customTable =
        DATATABLE (
            "RegionCountryName", STRING,
            "City", STRING,
            {
                { "United States", "New York" },
                { "United States", "Los Angeles" },
                { "United States", "Miami" },
                { "United States", "Atlanta" },
                { "Germany", "Berlin" },
                { "Germany", "Frankfurt" }
            }
        )
    VAR geoTable =
        SELECTCOLUMNS (
            Geography,
            "Geography[RegionCountryName]", Geography[RegionCountryName],
            "ContinentName", Geography[ContinentName]
        )
    VAR filterCountriesWithLineage =
        TREATAS (
            DISTINCT (
                SELECTCOLUMNS ( customTable, "Geography[RegionCountryName]", [RegionCountryName] )
            ),
            Geography[RegionCountryName]
        )
     VAR filterCountriesNoLineage =
            DISTINCT (
                SELECTCOLUMNS ( customTable, "Geography[RegionCountryName]", [RegionCountryName] )
            )

// A withFilter (TREATAS not required as lineage is there with specifing RegionContryName)
EVALUATE
DISTINCT ( FILTER ( geoTable, [RegionCountryName] IN filterCountries ) )

// B with Join Requires data lineage
EVALUATE
NATURALINNERJOIN(geoTable, filterCountriesWithLineage)

```
7. Create a custom table SpecialDiscount with two Rows 
    + StartDate (DATETIME) 1/1/2013 And EndDate (DATETIME) 1/6/2013 AM And Discount (DOUBLE) 0.10
    + StartDate (DATETIME) 1/6/2013 And EndDate (DATETIME) 31/12/2013 And Discount (DOUBLE) 0.30 <br>
    <br>
	StartDate (DATETIME) 1/6/2013 And EndDate (DATETIME) 31/12/2013 And Discount (DOUBLE) 0.30
	Calculate a table that gives any order a special discount, that has not already got a discount via a promotion
```dax
DEFINE
    VAR tblSpecialDiscount =
        DATATABLE (
            "StartDate", DATETIME,
            "EndDate", DATETIME,
            "SpecialDiscount", DOUBLE,
            {
                { "1/1/2013", "1/6/2013", 0.1 },
                { "1/7/2013", "31/12/2013", 0.3 }
            }
        )


EVALUATE
ADDCOLUMNS (
    CALCULATETABLE (
        SUMMARIZECOLUMNS (
            Sales[SalesKey],
            Stores[StoreName],
            'Calendar'[DateKey],
            Promotion[PromotionName],
            Promotion[DiscountPercent],
            "@SalesAmountNoDiscount", SUM ( Sales[SalesAmount] )
        ),
        'Calendar'[Year] = 2013
    ),
    
    "SalesAmountStandDiscount",
        ( 1 - Promotion[DiscountPercent] ) * [@SalesAmountNoDiscount],
    
    "SalesAmountSpecialDiscount",
        IF (
            Promotion[DiscountPercent] <> 0,
            0,
            MAXX (
                FILTER( 
                    tblSpecialDiscount,
                    [StartDate] <= 'Calendar'[DateKey] &&
                    [EndDate] >= 'Calendar'[DateKey]
                ),
                (1-[SpecialDiscount]) * [@SalesAmountNoDiscount]
            )
        )
) )
)
```
