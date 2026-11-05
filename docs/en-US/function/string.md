#### function: string
Syntax: string(<data> [format <format_pattern>])
Return: string. Converts <data> to a string, optionally with formatting.
Parameter **<data>**: data to be converted, can be multiple data types. Required parameter; multiple types; parameter name omitted.
> Example: Convert float 3456.78 to string.
SQLazy snippet: string(3456.78) // result is "3456.78"
Parameter **[format <format_pattern>]**: pattern used for formatting during conversion. Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Convert float 3456.78 to string, prepend "$", use thousands separators, keep 2 decimal places.
SQLazy snippet: string(3456.78; format "$#,##0.00") // result is "$3,456.78"