#### function: asc
Syntax: asc(<character>)
Return: If <character> is an 8-bit character (e.g., English and extended), return ASCII code; if it is a 16-bit character (e.g., CJK Asian characters), return Unicode value.
Parameter **<character>**: character. Required parameter; character type; parameter name omitted.
> Example: ASCII code of "d".
NLC snippet: asc("d") // result is 100
> Example: Unicode value of "中".
NLC snippet: asc("中") // result is 20013