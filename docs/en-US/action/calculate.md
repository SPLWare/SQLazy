### Action: calculate
Syntax: <expression>
Parameter: **expression**
An expression that evaluates to a simple data type (including constants). Required parameter; parameter name must be omitted.
> Example: calculate the formula 3+10-5
NLC: calculate 3+10-5
> Example: define a date set using the expression [2024-01-03, 2024-01-22,2024-01-05,2024-01-04]
NLC: calculate [2024-01-03, 2024-01-22,2024-01-05,2024-01-04]
> Example: please calculate the expression ((Order_example_table find 301).Amount)
NLC: calculate ((Order_example_table find 301).Amount)