#### function: mod
Syntax: mod(<dividend> <divisor>)
Return: remainder computed from <dividend> and <divisor>.
Parameter **<dividend>**: dividend/original value. Required parameter; real type; parameter name omitted.
Parameter **<divisor>**: divisor/the range to constrain. Required parameter; real type; parameter name omitted.
> Example: Compute the remainder of -450 divided by 360.
SQLazy snippet: mod(-450,360) // result is -90
> Example: Compute 10 modulo 3.1.
SQLazy snippet: mod(10,3.1) // result is 0.7