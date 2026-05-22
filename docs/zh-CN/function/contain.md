#### function: contain
Syntax: <object_parameter> contain(<data>)
Return: boolean. Returns true if the set <object_parameter> contains <data> as a member, otherwise false.
Parameter **<object_parameter>**: a set. Required parameter; type is set; parameter name omitted.
Parameter **<data>**: data to compare with set members. Required parameter; type is expression; parameter name omitted.
> Example: Check whether [30,100,200] contains 100.
NLC snippet: [30,100,200] contain(100) // returns true.