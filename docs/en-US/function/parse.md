#### function: parse
Syntax: (<object_parameter> parse([match_quote] [escape]))
Return: multiple data types. Parses <object_parameter> into an appropriate data type, such as date, time, integer, float. By default, parses the entire string. Optionally parse only quoted content. By default, does not parse escape characters; optionally parse escape characters.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
> Example: Parse the string "10:20:30" into the appropriate data type.
NLC snippet: ("10:20:30" parse) // result is time type 10:20:30
Parameter **match_quote**: If <object_parameter> starts with a quote, parse until the closing quote and stop; return the string.
> Example: Parse the string "\'sadsdxc\'+23", taking only the quoted part.
NLC snippet: ("\'sadsdxc\'+23" parse(match_quote)) // returns "\'sadsdxc\'"
Parameter **escape**: Parse escape characters, supporting non-printable characters and Unicode escapes.
> Example: Parse "a\tb", including escape characters.
NLC snippet: ("a\tb" parse(escape)) // result is "a\tb"
> Example: Parse "\u4e2d\u56fd", including escape characters.
NLC snippet: ("\u4e2d\u56fd" parse(escape)) // result is "中国"