#### function: string
Syntax: string(<data> [format <format_pattern>] [escape] [quote] [language <language_name>])
Return: string. Converts <data> to a string, optionally with formatting, escape characters, surrounding quotes, and language for time types.
Parameter **<data>**: data to be converted, can be multiple data types. Required parameter; multiple types; parameter name omitted.
> Example: Convert float 3456.78 to string.
NLC snippet: string(3456.78) // result is "3456.78"
Parameter **[format <format_pattern>]**: pattern used for formatting during conversion. Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Convert float 3456.78 to string, prepend "$", use thousands separators, keep 2 decimal places.
NLC snippet: string(3456.78; format "$#,##0.00") // result is "$3,456.78"
Parameter **escape**: Ignores parameter [format <format_pattern>] and escapes non-printable characters in <data>. Displays tab, carriage return, line feed, etc. as escape sequences; if single quotes, double quotes, or backslashes are present, they are escaped. Non-required parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.
> Example: Convert "a\tb" to string, turning the invisible tab into an explicit escape sequence.
NLC snippet: string("a\tb"; escape) // result is "a\tb"
Parameter **[quote]**: Ignores parameter [format <format_pattern>] and adds double quotes around the output string.
> Example: Add double quotes around the string "a\tb".
NLC snippet: string("a\tb"; quote) // result is "\"a\tb\""
Parameter **[language <language_name>]**: language name, only effective when outputting dates/times. Case-insensitive. Common language names: zh (Chinese), en (English). Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Convert the date 2009-02-23 to string with pattern "MMM dd, yyyy" and language English.
NLC snippet: string(2009-02-23; format "MMM dd, yyyy"; language "en") // result is "Feb 23, 2009"