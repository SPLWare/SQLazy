#### function: islower
Syntax: <object_parameter> islower
Return: boolean. Returns true if all characters in <object_parameter> are lowercase letters, otherwise false.
Parameter **<object_parameter>**: string to be checked. Required parameter; type is string; parameter name omitted.
> Example: Check if "abc" consists entirely of lowercase letters.
NLC snippet: "abc" islower // result is true
> Example: Check if "Aa#" consists entirely of lowercase letters.
NLC snippet: "Aa#" islower // result is false