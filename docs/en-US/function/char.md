#### function: char
Syntax: char(<integer>)
Return: If <integer> is an 8-bit integer, return the corresponding ASCII character (e.g., English and extended); if it is a 16-bit integer, return the corresponding Unicode character (e.g., CJK Asian characters).
Parameter **<integer>**: Required parameter; integer type; parameter name omitted.
> Example: ASCII character for integer 100.
NLC snippet: char(100) // result is the character "d"
> Example: Unicode character for integer 20013.
NLC snippet: char(20013) // result is the character "中"