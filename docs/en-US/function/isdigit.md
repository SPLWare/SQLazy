#### function: isdigit
Syntax: <object_parameter> isdigit
Return: boolean. Returns true if all characters in <object_parameter> are digits, otherwise false.
Parameter **<object_parameter>**: string to be checked. Required parameter; type is string; parameter name omitted.
> Example: Check if "123" consists entirely of digits.
NLC snippet: "123" isdigit // result is true
> Example: Check if "12a3" consists entirely of digits.
NLC snippet: "12a3" isdigit // result is false