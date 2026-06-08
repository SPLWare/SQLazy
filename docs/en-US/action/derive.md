### Action: derive
Syntax: [append] {[<expression>] [as <new_field_name>] } [delete <field_list>] [distinct]
Parameters: [<expression>] [as <new_field_name>]
The two parameters usually appear together, used to calculate and generate a new column in the new table. The expression can be a field name of the original table (a type of expression), or an expression based on one or more fields, or a constant or null (a type of expression). This parameter does not support cross-row calculation and aggregation calculation, i.e., the expression cannot contain relative position expressions like F[i] or relative interval expressions like F[a:b], nor aggregate calculations such as sum or average of a set. The as parameter represents the new field name. {} indicates repetition, meaning that each pair of parameters [<expression>] and [as <new_field_name>] represents one field in the new table. Both parameters are optional: when the expression is omitted, the value of this column is null (in which case as parameter must be present); when the as parameter is omitted, an automatic name is generated (in which case the expression must be present); the parameter name of the expression must be omitted, the parameter name of as cannot be omitted.
> Example: generate a new table from Order_example_table, copy the OrderID field as is, rename the Amount field to OriginalAmount, generate a new field using the expression (Amount*1.1), with an automatically generated name.
NLC: derive OrderID , Amount as OriginalAmount, Amount*1.1
Result:
OrderID	OriginalAmount	Amount*1.1
1	440	484
2	1863.4	2049.74
4	670.8	737.88
5	3730	4103
6	1444.8	1589.28
Parameter: [append]
Default does not copy the original table. When this parameter is present, it means copying the original table and calculating to generate new columns, i.e., appending new columns to the original table, equivalent to a simplified compute action. Optional parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: append new columns to Order_example_table, rename the Amount field to OriginalAmount, generate a new field using the expression (Amount*1.1), with an automatically generated name.
NLC: derive append Amount as OriginalAmount, Amount*1.1
Result:
OrderID	Amount	OrderDate	OriginalAmount	Amount*1.1
1	440	2021-01-01	484	440	484
2	1863.4	2021-01-02	1863.4	2049.74
4	670.8	2021-01-03	670.8	737.88
5	3730	2021-01-04	3730	4103
6	1444.8	2021-01-05	1444.8	1589.28
Parameter: [delete <field_list>]
Used together with the [append] parameter, meaning deleting some unnecessary fields when copying the original table. Optional parameter; type is a set of field identifiers; parameter name cannot be omitted.
> Example: delete the OrderDate from Order_example_table and append new columns, rename the Amount field to OriginalAmount, generate a new field using the expression (Amount*1.1), with an automatically generated name.
NLC: derive append Amount as OriginalAmount, Amount*1.1; delete OrderDate
Result:
OrderID	Amount	OriginalAmount	Amount*1.1
1	440	484	440	484
2	1863.4	1863.4	2049.74
4	670.8	670.8	737.88
5	3730	3730	4103
6	1444.8	1444.8	1589.28
Parameter: **distinct**
After generating the new table, whether to deduplicate the records, keeping only unique records. Optional parameter; boolean type (parameter name cannot be omitted, no parameter value); default is no deduplication. Note that because the original table is usually duplicate-free, deduplication during append is also usually ineffective.