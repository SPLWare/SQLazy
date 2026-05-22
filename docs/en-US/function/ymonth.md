#### function: ymonth
Syntax: ymonth(<date>)
Return: integer. Extracts the year and month from <date> and returns year*100 + month.
Parameter **<date>**: original value. Required parameter; date type; parameter name omitted.
> Example: Extract year and month from field OrderD.
NLC snippet: ymonth(OrderD) // result similar to 202601