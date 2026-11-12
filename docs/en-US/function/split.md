#### function: split
Syntax: (<object_parameter> split <segment_number> [by <delimiter_string>])
Return: string. Splits <object_parameter> by <delimiter_string> and returns the segment numbered <segment_number>. Segment numbers start from 1; returns null when the number exceeds the actual number of segments. The delimiter defaults to a comma; when it is an empty string, the string is split into single characters. An empty segment produced by two consecutive delimiters still occupies a segment number, and whitespace inside a segment is kept as is.
Parameter **<object_parameter>**: the string to be split. Required parameter; string type; parameter name omitted.
Parameter **<segment_number>**: which segment to return, counting from 1. Required parameter; integer type or expression; the parameter name must be omitted.
Parameter **[by <delimiter_string>]**: the delimiter used for splitting. Optional parameter; string type; the parameter name cannot be omitted. Defaults to a comma; when an empty string, the string is split into single characters.
> Example: Split "a,b,c" by commas and take the 2nd segment.
SQLazy snippet: ("a,b,c" split 2)	// returns "b"
> Example: Split "a;b;c" by semicolons and take the 3rd segment.
SQLazy snippet: ("a;b;c" split 3, by ";")	// returns "c"
> Example: The segment number is an expression and the delimiter has two characters.
SQLazy snippet: ("a::b::c" split 1+1, by "::")	// returns "b"
> Example: Split "abc" into single characters and take the 2nd one.
SQLazy snippet: ("abc" split 2, by "")	// returns "b"
> Example: Two consecutive commas produce an empty segment, which still occupies a segment number.
SQLazy snippet: ("a,,b" split 2)	// returns the empty string ""
> Example: The segment number exceeds the actual number of segments.
SQLazy snippet: ("a,b" split 5)	// returns null
