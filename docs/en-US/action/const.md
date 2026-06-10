### Action: const
Syntax: struct<set_of_field_names> [data<set_or_expression_evaluating_to_set>]
Parameter: **struct**
Multiple field names used to define the table structure; required parameter; type is identifier set; parameter name cannot be omitted.
> Example: define an empty Order_example_table with 3 fields (no records or data) 
NLC: const struct OrderID, ClientID, OrderDate, Amount
Parameter: **data**
Multiple field values, to be filled into the table sequentially in row-major order; optional parameter; set type, including expressions that evaluate to a set; parameter name cannot be omitted. Note, without this parameter, an empty table with structure is generated; the number of set members does not have to be an integer multiple of the number of fields, unfilled cells are filled with "null".
> Example: define an Order_example_table with 3 fields and 2 records.
NLC: const struct OrderID, ClientID, OrderDate, Amount; data 1001, "SVF", 2021-01-01, 3131.34, 1002, "VNV", 2021-01-03, 2121.8

