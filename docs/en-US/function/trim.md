#### function: trim
Syntax: (<object_parameter> trim [left] [right])
Return: string. Removes whitespace from both sides of <object_parameter> and returns the result. Optionally trim only left or only right.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
> Example: Trim whitespace from both sides of " a bc ".
NLC snippet: (" a bc " trim) // result is "a bc"
Parameter **[left]**: By default, trim both sides. This parameter trims only the left side. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Trim left whitespace from " a bc ".
NLC snippet: (" a bc " trim left) // result is "a bc "
Parameter **[right]**: Similar to [left]; trims only the right side.
