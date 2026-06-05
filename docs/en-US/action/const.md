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

This action has an overload with the same name but different parameters (conceptually similar to function overloading but not a function), as follows
Syntax 2: {[<expression>] [as <field_name>]}
Parameters
The two parameters typically appear together, representing the value and field name of a field, repeated multiple times to form one or more records; parameter "[<expression>]" is optional, parameter "as" is required; parameter "[<expression>]" type is expression, including constants, parameter "as" type is identifier, multiple pairs form a "value-key" table; parameter "[<expression>]" must omit the parameter name, parameter "as" cannot omit the parameter name. Note, the field name of the first record must be written, from the second record onward it may be omitted. There are two forms: with line breaks and without. The line-break form uses the line break as a marker, each line is a record.
> Example, generate an Order_example_table with 4 fields and 2 records using constants. NLC:
NLC: const 1001 as OrderID, "SVF" as ClientID, 2021-01-01 as OrderDate, 3131.34 as Amount,
1002, "VNV", 2021-01-03, 2121.8
Explanation: The above is the line-break form. The non-line-break form uses the number of field names as the marker, each repetition of field names is one record, including omitted field names.
NLC:
const 1001 as OrderID, "SVF" as ClientID, 2021-01-01 as OrderDate, 3131.34 as Amount, 1002, "VNV", 2021-01-03, 2121.8
