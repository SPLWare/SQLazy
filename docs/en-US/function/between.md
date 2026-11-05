#### function: between
Syntax: (<object_parameter> between <lower_bound> <upper_bound> [left_open] [right_open])
Return value: boolean.
Parameter **<object_parameter>**: original value/original expression. Required parameter; type is numeric, date, time; parameter name omitted.
Parameter **<lower_bound>** **<upper_bound>**: These two parameters define the start and end of the interval. If the object parameter is within the interval, return true; otherwise false. Both required; types are numeric, date, time; parameter names omitted.
> Example: Determine whether the order_amount field is between 1000 and 10000.
SQLazy snippet: (order_amount between 1000, 10000) // returns true
Parameter **[left_open]**: Default is closed on the left. This parameter indicates an open left bound. Non-required parameter; type is boolean; parameter value omitted, parameter name cannot be omitted.
Parameter **[right_open]**: Default is closed on the right. This parameter indicates an open right bound. Non-required parameter; type is boolean; parameter value omitted, parameter name cannot be omitted.
> Example: Determine whether 1000 is in the left-open, right-closed interval between 1000 and 10000.
SQLazy snippet: (1000 between 1000, 10000; left_open) // returns false