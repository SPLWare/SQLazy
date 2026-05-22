#### function: icount
Syntax: icount({<data>})
Return value: numeric. Returns the count of multiple expressions; duplicate data is counted only once, i.e., distinct count.
Parameter: **<data>** original value/original expression. Usually multiple parameters used together, i.e., number of parameters >= 1. Required parameter; type is simple data type; parameter name omitted.
> Example: Distinct count of "Class 2", "Class 3", "Class 3".
NLC snippet: icount("Class 2","Class 3","Class 3") // returns 2