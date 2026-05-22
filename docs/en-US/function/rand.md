#### function: rand
Syntax: rand(<range>)
Return: Generate a random number based on <range>.
Parameter **<range>**: When <range> is an integer, generate a random integer between 0 and <range>-1; when <range> is omitted, generate a random decimal between 0 and 1. Non-required parameter; numeric type; parameter name omitted.
> Example: Generate a random integer between 0 and 100.
NLC snippet: rand(101)
> Example: Generate a random decimal between 0 and 1.
NLC snippet: rand()