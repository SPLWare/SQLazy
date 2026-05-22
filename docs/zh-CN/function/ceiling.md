#### function: ceiling
Syntax: ceiling(<original_value> <digits>)
Return: Round <original_value> up to <digits>; any remainder causes a carry.
Parameter **<original_value>**: the number to be rounded up. Required parameter; real type; parameter name omitted.
Parameter **<digits>**: number of digits to round to. Default is 0. For >0, digits to the right of the decimal point; for <0, digits to the left. Non-required parameter; integer type; parameter name omitted.
> Example: Round 35.52 up.
NLC snippet: ceiling(35.52) // result is 36 (digits default to 0).
> Example: Round 35.52 up to 1 digit.
NLC snippet: ceiling(35.52,1) // result is 35.6.
> Example: Round 35.52 up to -1 digit.
NLC snippet: ceiling(35.52,-1) // result is 40.