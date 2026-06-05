#### function: if
Syntax: if ({[<condition>] [then <true_expression>]} [else <false_expression>])
Return value: simple data type.
Parameter **[<condition>]**: If the result of this parameter is true, return the result of parameter [then <true_expression>]. Required parameter; type is conditional expression; parameter name omitted. Note that this parameter is usually used together with parameter [then <true_expression>], but can also be used alone.
Parameter **[then <true_expression>]**: If the result of parameter [<condition>] is true, return the result of this parameter. Non-required parameter; type is expression; parameter name **then** cannot be omitted. Note that this parameter must be used together with parameter [<condition>].
> Example: If the year of the order_date field is 2019 and order_amount > 10000, return "2019 large order".
NLC snippet: if ((year(order_date)=2019 and order_amount>10000) then "2019 large order")

Multiple pairs of [<condition>] and [then <true_expression>] parameters are allowed. They should be evaluated sequentially: first evaluate the first [<condition>]; if true, return the first [then <true_expression>]; if false, evaluate the second [<condition>]; if true, return the second [then <true_expression>]; and so on.
> Example: If year(order_date)=2019 and order_amount>10000, return "2019 large order"; if year(order_date)=2019 and order_amount>2000, return "2019 medium order"; if year(order_date)=2019 and order_amount<=2000, return "2019 small order".
NLC snippet: if ((year(order_date)=2019 and order_amount>10000) then "2019 large order", (year(order_date)=2019 and order_amount>2000) then "2019 medium order", (year(order_date)=2019 and order_amount<=2000) then "2019 small order")

Parameter **[else <false_expression>]**: If all preceding **[<condition>]** are false, return this parameter. Non-required parameter; type is expression; parameter name **else** cannot be omitted.
> Example: If year(order_date)=2019 and order_amount>10000, return "2019 large order"; if year(order_date)=2019 and order_amount>2000, return "2019 medium order"; if year(order_date)=2019 and order_amount<=2000, return "2019 small order"; otherwise, return "other year orders".
NLC snippet: if ((year(order_date)=2019 and order_amount>10000) then "2019 large order", (year(order_date)=2019 and order_amount>2000) then "2019 medium order", (year(order_date)=2019 and order_amount<=2000) then "2019 small order"; else "other year orders")
