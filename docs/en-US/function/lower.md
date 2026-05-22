#### function: lower
Syntax: lower(<string> [skip_quoted])
Return: Converts all uppercase letters in <string> to lowercase.
Parameter **<string>**: string to be converted. Required parameter; type is string; parameter name omitted.
Parameter **[skip_quoted]**: When present, characters inside quotes are not converted. Non-required parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.
> Example: Convert "ABC\"ABC\"#" to lowercase.
NLC snippet: lower("ABC\"ABC\"#") // result is "abc\"abc\"#"
> Example: Convert "ABC\"ABC\"#" to lowercase, but do not convert characters inside quotes.
NLC snippet: lower("ABC\"ABC\"#" skip_quoted) // result is "abc\"ABC\"#"