#### function: getsubs
Syntax: (<object_parameter> getsubs <marker_substring> [nocase] [reverse] [skip_quoted] [previous])
Return: string. If <object_parameter> contains <marker_substring>, return the substring after <marker_substring>. Returns empty if no substring follows or if <marker_substring> is not found. Optionally case-insensitive, reverse search, skip quoted content, or return the substring before <marker_substring>.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<marker_substring>**: substring to match; by default, the function returns the substring after this parameter. Required parameter; string type; parameter name omitted.
> Example: Find the substring after "人民" in "中国人民电视台".
NLC snippet: ("中国人民电视台" getsubs "人民") // returns "电视台".
Parameter **nocase**: Default is case-sensitive. This parameter ignores case. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Find substring after "Def" in "abcdefghi", case-insensitive.
NLC snippet: ("abcdefghi" getsubs "Def"; nocase) // returns "ghi"
Parameter **reverse**: Search from the end instead of the beginning. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
Parameter **skip_quoted**: By default, search inside quotes; with this parameter, skip quoted content. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Find substring after "人民" in "中国\'人民广播\'电视台", skipping quoted content.
NLC snippet: ("中国\'人民广播\'电视台" getsubs "人民"; skip_quoted) // returns null
Parameter **previous**: By default, returns the substring after <marker_substring>. With this parameter, returns the substring before <marker_substring>. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Find substring before "人民" in "中国人民电视台".
NLC snippet: ("中国人民电视台" getsubs "人民"; previous) // returns "中国".