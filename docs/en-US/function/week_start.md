#### function: week_start
Syntax: <object_parameter> week_start ([monday_first])
Return: first day of the week containing the date <object_parameter>.
Parameter **<object_parameter>**: Required parameter; type is date or datetime; parameter name omitted.
> Example: First day of the week containing 1980-02-27.
NLC snippet: 1980-02-27 week_start // result is date 1980-02-24
Parameter **[monday_first]**: By default, Sunday is the first day of the week. With this parameter, Monday is the first day.
> Example: First day of the week containing 1980-02-27, with Monday as first day.
NLC snippet: 1980-02-27 week_start(monday_first) // result is date 1980-02-25