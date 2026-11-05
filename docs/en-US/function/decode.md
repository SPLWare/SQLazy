#### function: decode
Syntax: (<object_parameter> decode <key_value_list> [<default_value>] [reverse])
Return: Looks up <object_parameter> by key in <key_value_list> and returns the matching value; returns <default_value> if nothing matches. When compiled to SQL it can be implemented with DECODE or CASE WHEN.
Parameter **<object_parameter>**: the value to be converted. Required parameter; type is simple data type; parameter name omitted.
Parameter **<key_value_list>**: a list with keys and values alternating in pairs, e.g. ["A","unpaid","B","paid"]. Required parameter; set type; the parameter name must be omitted.
Parameter **<default_value>**: value returned when nothing matches. Optional parameter; type is simple data type; the parameter name must be omitted. Note, when this parameter is omitted and <key_value_list> has an odd number of members, the last member is used as the default value automatically.
Parameter **[reverse]**: Swaps the order of keys and values, i.e. looks up in the "value to key" direction. Optional parameter; boolean type; the parameter name cannot be omitted, the parameter value must be omitted.
> Example: In the order status field, A means unpaid and B means paid; convert it into words.
SQLazy snippet: (order_status decode ["A", "unpaid", "B", "paid"]) // A returns "unpaid", B returns "paid"
> Example: The same conversion, but all other statuses return "unknown".
SQLazy snippet: (order_status decode ["A", "unpaid", "B", "paid"]; "unknown") // other statuses return "unknown"
> Example: The default value is omitted and the list has an odd number of members; the last member becomes the default.
SQLazy snippet: (order_status decode ["A", "unpaid", "B", "paid", "unknown"]) // other statuses return "unknown"
