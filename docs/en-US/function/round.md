#### function: round
Syntax: round(<original_value> <digits>)
Return: Round <original_value> to <digits>.
Parameter **<original_value>**: the number to be rounded. Required parameter; real type; parameter name omitted.
Parameter **<digits>**: number of digits to round to. Default is 0. For >0, digits to the right of the decimal point; for <0, digits to the left. Non-required parameter; integer type; parameter name omitted.
> Example: Round 35.52.
NLC snippet: round(35.52) // result is 36 (digits default to 0).
> Example: Round 35.52 to 1 digit.
NLC snippet: round(35.52, 1) // result is 35.5
> Example: Round 35.52 to -1 digit.
NLC snippet: round(35.52,-1) // result is 40