#### function: rands
Syntax: rands([from <source_string>] [length <result_length>])
Return: string. Randomly selects characters from [from <source_string>] to form a string of length [length <result_length>].
Parameter **[from <source_string>]**: source of random characters. Required parameter; string type; parameter name omitted.
Parameter **[length <result_length>]**: length of the generated random string. Required parameter; integer type; parameter name omitted.
> Example: Randomly generate a new string of length 5 using characters from "abc".
NLC snippet: rands(from "abc", length 5) // result random, e.g., "abcac"