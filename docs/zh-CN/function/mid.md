#### function: mid
Syntax: mid(<source_string> <start_position> <length>)
Return: string. Returns the substring of <source_string> starting from the <start_position>-th character from the left, with length <length>.
Parameter **<source_string>**: Required parameter; string type; parameter name omitted.
Parameter **<start_position>**: Required parameter; integer type; parameter name omitted.
Parameter **<length>**: Required parameter; integer type; parameter name omitted.
> Example: Substring starting from the 3rd character of "中国人民电视台" with length 2.
NLC snippet: mid("中国人民电视台",3,2) // result is "人民"