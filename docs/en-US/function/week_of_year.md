#### function: week_of_year
Syntax: <object_parameter> week_of_year([monday_first])
Returns: The week number of the year for the date <object_parameter>. January 1st is week 1. By default, Sunday is the first day of the week, and the week increments on Sunday.
Parameter <object_parameter>: Mandatory parameter; type is date or datetime; parameter name is omitted.
> Example: Calculate the week number of the year for 1980-02-27
NLC snippet: 1980-02-24 week_of_year //Result is 9
Parameter [monday_first]: By default, Sunday is the first day of the week, and the week increments on Sunday. If this parameter is present, Monday is considered the first day of the week, and the week increments on Monday. Optional parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.
> Example: Calculate the week number of the year for 1980-02-27, with Monday as the first day of the week
NLC snippet: 1980-02-24 week_of_year(monday_first) //Result is 8
