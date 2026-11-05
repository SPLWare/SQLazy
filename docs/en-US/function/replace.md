#### function: replace
Syntax: (<object_parameter> replace [from <marker_substring>] [to <new_substring>])
Return: string. Replaces all occurrences of [from <marker_substring>] with [to <new_substring>] in <object_parameter> and returns the result.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **[from <marker_substring>]**: substring to be replaced. Required parameter; string type; parameter name omitted.
Parameter **[to <new_substring>]**: new substring to replace with. Required parameter; string type; parameter name omitted.
> Example: Replace all occurrences of "a" with "China" in "abcabcdef".
SQLazy snippet: ("abcabcdef" replace from "a", to "China") // result is ChinabcChinabcdef