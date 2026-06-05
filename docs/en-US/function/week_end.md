#### function: week_end
Syntax: <object_parameter> week_end [monday_first]
Return: last day of the week containing the date <object_parameter>.
Parameter **<object_parameter>**: Required parameter; type is date or datetime; parameter name omitted.
> Example: Last day of the week containing 1980-02-27.
NLC snippet: (1980-02-27 week_end) // result is date 1980-03-01
Parameter **[monday_first]**: By default, Sunday is the first day of the week. With this parameter, Monday is the first day.
> Example: Last day of the week containing 1980-02-27, with Monday as first day.
NLC snippet: (1980-02-27 week_end monday_first) // result is date 1980-03-02
