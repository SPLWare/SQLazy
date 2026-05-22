#### function: upper
Syntax: upper(<string> [skip_quoted])
Return: Converts all lowercase letters in <string> to uppercase.
Parameter **<string>**: string to be converted. Required parameter; type is string; parameter name omitted.
Parameter **[skip_quoted]**: When present, characters inside quotes are not converted. Non-required parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.
> Example: Convert "abc\"abc\"#" to uppercase.
NLC snippet: upper("abc\"abc\"#") // result is "ABC\"ABC\"#"
> Example: Convert "abc\"abc\"#" to uppercase, but do not convert characters inside quotes.
NLC snippet: upper("abc\"abc\"#" skip_quoted) // result is "ABC\"abc\"#"