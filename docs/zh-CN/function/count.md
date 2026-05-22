#### function: count
Syntax: count({<data>})
Return value: numeric. Returns the count of multiple expressions. Duplicate data must be counted multiple times, i.e., counting with duplicates.
Parameter: **<data>** original value/original expression. Usually multiple parameters used together, i.e., number of parameters >= 1. Required parameter; type is simple data type; parameter name omitted.
> Example: Count "Class 2", "Class 3", "Class 3".
NLC snippet: count("Class 2","Class 3","Class 3") // returns 3