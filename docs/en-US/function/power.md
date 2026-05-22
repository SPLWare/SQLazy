#### function: power
Syntax: power(<base>,<exponent>)
Return: Compute power using <base> and <exponent>. When exponent is 1/N, it can compute roots.
Parameter **<base>**: base. Required parameter; real type; parameter name omitted.
Parameter **<exponent>**: exponent. Required parameter; real type; parameter name omitted.
> Example: 2 to the 4th power.
NLC snippet: power(2,4) // result is 16
> Example: 16 to the 0.25 power, i.e., the 4th root of 16.
NLC snippet: power(16,0.25) // result is 2