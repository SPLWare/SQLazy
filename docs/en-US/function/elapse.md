#### function: elapse
Syntax: (<object_parameter> elapse <period> <unit>)
Return: time type. Returns a new time point after moving from <object_parameter> by a period.
Parameter **<object_parameter>**: original time point, can be date, time, or datetime. Required parameter; string type; parameter name omitted.
Parameter **<period>**: time period to move. Positive values move forward, negative values move backward. Default unit is day.
> Example: Move 5 days from 2020-02-15.
NLC snippet: (2020-02-15 elapse 5) // result is 2020-02-20
Parameter **<unit>**: unit of <period>; default is day. Use this parameter to set other units. Non-required parameter; enumeration type; enumeration values: year, quarter, month, week, day, hour, minute, second; parameter name omitted.
> Example: Move 5 months from 2020-02-15.
NLC snippet: (2020-02-15 elapse 5 month) // result is 2020-07-20
