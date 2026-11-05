#### function: getsubs
Syntax: (<object_parameter> getsubs <marker_substring> [nocase])
Return: string. If <object_parameter> contains <marker_substring>, return the substring after <marker_substring>. Returns an empty string "" if no substring follows or if <marker_substring> is not found. Optionally case-insensitive.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<marker_substring>**: substring to match; by default, the function returns the substring after this parameter. Required parameter; string type; parameter name omitted.
> Example: Find the substring after "人民" in "中国人民电视台".
SQLazy snippet: ("中国人民电视台" getsubs "人民") // returns "电视台".
Parameter **nocase**: Default is case-sensitive. This parameter ignores case. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Find substring after "Def" in "abcdefghi", case-insensitive.
SQLazy snippet: ("abcdefghi" getsubs "Def"; nocase) // returns "ghi"
