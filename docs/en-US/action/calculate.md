### Action: calculate
Syntax: <expression>
Parameter: **expression**
An expression that evaluates to a simple data type (including constants). Required parameter; parameter name must be omitted.
> Example: calculate a formula 3+10-5
NLC: calculate 3+10-5
> Example: define the value of a date constant
NLC: calculate 2024-01-03
> Example: use code 301 to look up the corresponding amount in the order_example_table
NLC: calculate (order_example_table code_find 301)
