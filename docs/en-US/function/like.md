#### function: like
Syntax: (<object_parameter> like(<pattern> [sql_match]))
Return: boolean. Returns true if <object_parameter> matches the <pattern> containing wildcards, otherwise false.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<pattern>**: string containing wildcards. Default wildcards: * (zero or more characters), ? (one character). Required parameter; string type; parameter name omitted.
> Example: Does "abc123" match the pattern "123*"?
NLC snippet: ("abc123" like("123*")) // returns true
> Example: Does "abc123" match the pattern "123?"?
NLC snippet: ("abc123" like("123?")) // returns false
Parameter **[sql_match]**: Default wildcards are *, ?. With this parameter, switch to SQL wildcards: % (zero or more characters), _ (one character).
> Example: Does "abc123" match the pattern "123_", using SQL wildcards?
NLC snippet: ("abc123" like("123_"; sql_match)) // returns true