#### function: fill
Syntax: fill(<source_string> <repeat_count>)
Return: string. Copies <source_string> <repeat_count> times and concatenates into a large string.
Parameter **<source_string>**: Required parameter; string type; parameter name omitted.
Parameter **[repeat_count]**: number of times to copy. Required parameter; integer type; parameter name omitted.
> Example: Copy "a b" 10 times.
NLC snippet: fill("a b",10) // result "a ba ba ba ba ba ba ba ba ba b"