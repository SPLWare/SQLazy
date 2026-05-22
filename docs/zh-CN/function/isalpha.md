#### function: isalpha
Syntax: <object_parameter> isalpha
Return: boolean. Returns true if all characters in <object_parameter> are letters from a-z or A-Z, otherwise false.
Parameter **<object_parameter>**: string to be checked. Required parameter; type is string; parameter name omitted.
> Example: Check if "abc" consists entirely of letters.
NLC snippet: "abc" isalpha // result is true
> Example: Check if "abc1" consists entirely of letters.
NLC snippet: "abc1" isalpha // result is false