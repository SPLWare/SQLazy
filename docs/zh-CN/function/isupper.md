#### function: isupper
Syntax: <object_parameter> isupper
Return: boolean. Returns true if all characters in <object_parameter> are uppercase letters, otherwise false.
Parameter **<object_parameter>**: string to be checked. Required parameter; type is string; parameter name omitted.
> Example: Check if "ABC" consists entirely of uppercase letters.
NLC snippet: "ABC" isupper // result is true
> Example: Check if "Aa#" consists entirely of uppercase letters.
NLC snippet: "Aa#" isupper // result is false