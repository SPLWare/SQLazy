#### function: chn
Syntax: (<object_parameter> chn([uppercase]))
Return: string. Converts <object_parameter> to a Chinese number string.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
> Example: Convert -97 to Chinese number.
NLC snippet: (-97 chn) // returns "负九十七"
Parameter **[uppercase]**: Default is lowercase Chinese numbers. This parameter indicates uppercase Chinese numbers. Non-required parameter; boolean type; parameter name omitted.
> Example: Convert -97 to uppercase Chinese number.
NLC snippet: (-97 chn(upper)) // returns "负玖柒"