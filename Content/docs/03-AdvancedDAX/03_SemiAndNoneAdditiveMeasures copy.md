# None-Additive and Semi-Additive Measures

## Semi-Additive-Measures

**Definition:** <br>
Can be summed across some dimensions, but not all. Balance amounts are common semi-additive facts because they are additive across all dimensions except time.

**Calculation:** <br>
```dax
LastBalanceAllCustomers := 
VAR LastDateAllCustomers = 
 CALCULATETABLE ( 
 LASTNONBLANK (
 'Date'[Date], 
 COUNTROWS ( RELATEDTABLE ( Balances ) ) 
 ),
 ALL ( Balances[Name] )
 )
VAR Result = 
 CALCULATE (
 SUM( Balances[Balance] ), 
 LastDateAllCustomers
 )
RETURN 
 Result
```
## None-Addive-Measures

**Definition:** <br>
Some measures are completely non-additive, such as ratios. A good approach for non-additive facts is, where possible, to store the fully additive components  of the non-additive measure and sum these components  into the ﬁnal answer set before calculating the ﬁnal non-additive fact.

**Calculation:** <br>
```dax
SalesPerWorkingDay = 
DIVIDE( [Sales Amount], [NumOfWorkingDays] )
```