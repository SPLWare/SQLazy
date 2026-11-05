#### function: like
Syntax: (<object_parameter> like <pattern> [case_insensitive])
Return: boolean. Returns true if <object_parameter> matches the <pattern> containing wildcards, otherwise false.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<pattern>**: string containing wildcards. Default wildcards: % (zero or more characters), _ (one character). Required parameter; string type; parameter name omitted.
> Example: Does "abc123" match the pattern "abc%"?
SQLazy snippet: ("abc123" like "abc%") // returns true
> Example: Does "abc123" match the pattern "abc_"?
SQLazy snippet: ("abc123" like "abc_") // returns false
Parameter **[case_insensitive]**: Default is case-sensitive. With this parameter the matching becomes case-insensitive. Optional parameter; boolean type; parameter name must not be omitted, parameter value must be omitted.
> Example: Does "abc123" match the pattern "ABC%", case-insensitive?
SQLazy snippet: ("abc123" like "ABC%"; case_insensitive) // returns true
