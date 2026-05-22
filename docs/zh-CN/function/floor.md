#### function: floor
Syntax: floor(<original_value> <digits>)
Return: Round <original_value> down to <digits>, discarding the remainder without carrying.
Parameter **<original_value>**: the number to be rounded down. Required parameter; real type; parameter name omitted.
Parameter **<digits>**: number of digits to round to. Default is 0. For >0, digits to the right of the decimal point; for <0, digits to the left. Non-required parameter; integer type; parameter name omitted.
> Example: Round 35.52 down.
NLC snippet: floor(35.52) // result is 35 (digits default to 0).
> Example: Round 35.52 down to 1 digit.
NLC snippet: floor(35.52,1) // result is 35.5.
> Example: Round 35.52 down to -1 digit.
NLC snippet: floor(35.52,-1) // result is 30.