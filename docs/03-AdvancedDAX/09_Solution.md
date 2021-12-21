# Lab 3: Solution

1. Create a Measure ProfitYTD with fiscal year from 01.06. - 31.5 and create a Measre ProfitPY (previous year) 

```dax
ProfitYTD = CALCULATE([Profit], DATESYTD('Calendar'[Date], "31-05"))
```
2. Illustrate the productCategory profit for 2013 with Year-over-Year value in $ and percent (incl. format)

```dax
ProfitYoY_Eur =
VAR varProfitActual = [Profit]
VAR varProfitPy = [ProfitPY]
VAR varYoY = varProfitActual - varProfitPy
RETURN
    IF ( NOT ISBLANK ( varYoY ), FORMAT ( varYoY, "Currency" ) )

ProfitYoY_Per = 
var varYoY = DIVIDE([ProfitYoY_Eur], [ProfitPY])
RETURN
    IF(not ISBLANK(varYoY), varYoY)    
```

3. Create the same output like in 2 by using calculation groups (Actual, YTD, PY, YoYEur, YoYPer

```dax
Actual =
    SELECTEDMEASURE ()

YTD = 
IF (
    ISSELECTEDMEASURE ( [Profit] ),
    CALCULATE ( SELECTEDMEASURE (), DATESYTD ( 'Calendar'[DateKey], "06-30" ) ),
    SELECTEDMEASURE ()
)

PY = 
IF (
    ISSELECTEDMEASURE ( [Profit] ),
    CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Calendar'[DateKey] ) ),
    SELECTEDMEASURE ()
)

YoY_Eur =
VAR varProfitActual =
    SELECTEDMEASURE ()
VAR varProfitPy =
    CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Calendar'[DateKey] ) )
VAR varYoY = varProfitActual - varProfitPy
RETURN
    varYoY

YoY_Per =
VAR varProfitActual =
    SELECTEDMEASURE ()
VAR varProfitPy =
    CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Calendar'[DateKey] ) )
VAR varYoY =
    DIVIDE ( [ProfitYoY_Eur], [ProfitPY] )
RETURN
    IF ( NOT ISBLANK ( varYoY ), FORMAT ( varYoY, "Percent" ) )

```

4. Create a running total measure YearCount, that counts up the years (2005=1, 2006=2,…). Display your result in a table

```dax
YearCount = 
COUNTROWS (
    FILTER (
        ALL ( 'Calendar'[Year] ),
        'Calendar'[Year] <= MAX ( 'Calendar'[Year] )
    )
)
```

5. Generate a custom table from 1 to 10. Create a Ranking Measures that rankes the Stores And ProductCategory and dynamically filter the top x rank based on your created table.

```dax

TopN = GENERATESERIES(1,10, 1)

RankProfit = 
VAR varTopN =
    SELECTEDVALUE ( 'TopN'[Value], 1 )
VAR varStoresProduct =
    TOPN (
        varTopN,
        CROSSJOIN (
            ALL ( Stores[StoreName] ),
            ALL ( ProductCategory[ProductCategory] )
        ),
        [Profit]
    )
VAR varRank =
    RANKX ( varStoresProduct, [Profit],, DESC )
RETURN
    IF (
        varRank <= varTopN,
        INT ( varRank ),
        CONCATENATE ( "Not Top ", VALUE ( varTopN ) )
    )

```

6. Use the table from task 5 and add the date on which the largest profit was achieved in the year or selected period 

```dax
TopProfitDDate = 
VAR varStoresProfits =
    SUMMARIZE ( Sales, Sales[cProfit], 'Calendar'[Date] )
VAR varTop =
    TOPN ( 1, varStoresProfits, [cProfit], DESC )
RETURN
    MAXX ( varTop, [Date] )
```

7. Calculate the percentage of profit of each sub-category in relation to the parent ProductCategory. On ProductCategory level this should result in 100%.

```dax
ProfitRatioHierarchy = 
VAR Profit = [Profit]
VAR totalProductCategory =
    CALCULATE ( [Profit], REMOVEFILTERS ( ProductSubcategory[ProductSubcategory] ) )
RETURN
    SWITCH (
        TRUE (),
        AND ( ISINSCOPE (  ProductSubcategory[ProductSubcategory] ), NOT ISBLANK ( Profit ) ), DIVIDE ( Profit, totalProductCategory ),
        AND ( ISINSCOPE (  'ProductCategory'[ProductCategory] ), NOT ISBLANK ( Profit ) ), DIVIDE ( Profit, profit ),
        BLANK ()
    )
```

8. Implement your own dynamic row level Security.

```dax
RLS_CountryRegion = 

DATATABLE (
    "User", STRING,
    "RegionCountryName", STRING,
    {
        { "m.riehle@oraylis.de",  "Germany" },
        { "m.riehle@oraylis.de", "United States" },
        { "m.mustermann@oraylis.de",  "Australia" },
        { "m.mustermann@oraylis.de",  "Italy" },
        { "m.mustermann@oraylis.de",  "United Kingdom" }
    }
)

//In Manage Roles Filter RLS_CountryRegion
[User] = USERPRINCIPALNAME ()

```