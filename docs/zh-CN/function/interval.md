#### function: interval
Syntax: interval([from <start_time>] [to <end_time>] [unit])
Return: integer. Returns the interval between [from <start_time>] and [to <end_time>]. Default unit is day; other units can be specified.
Parameter **[from <start_time>]**: start time point. Required parameter; type is date, time, or datetime; parameter name cannot be omitted.
Parameter **[to <end_time>]**: end time point. Required parameter; type is date, time, or datetime; parameter name cannot be omitted.
> Example: Compute days between date 1980-02-27 and date 1983-02-27 00:00:45.
NLC snippet: interval(from 1980-02-27, to 1983-02-27 00:00:45) // result is 1097
Parameter **[unit]**: Default interval unit is day. Can be set to other units. Non-required parameter; enumeration type; enumeration values: year, quarter, month, week, day, hour, minute, second, sunday, monday; parameter name omitted.
> Example: Compute whole years between 1980-02-27 and 1983-02-27 00:00:45.
NLC snippet: interval(from 1980-02-27, to 1983-02-27 00:00:45; year) // result is 3