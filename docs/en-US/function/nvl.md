#### function: nvl
Syntax: nvl({<data>})
Return value: data. Returns the first non-null and non-empty string data among multiple data/expressions.
Parameter: **<data>** original value/original expression. Usually multiple parameters used together, i.e., number of parameters >= 1. Required parameter; type is simple data type; parameter name omitted.
> Example: Take the first non-null and non-empty string among "", null, "Class 3".
NLC snippet: nvl("",null,"Class 3") // returns "Class 3"
