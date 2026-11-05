### Action: set
Syntax: [operation] [<operation_table>] [compare <column_name_for_comparison>]
Parameter: **operation** 
The type of set operation, parameter values (enum values) are: intersect, union, except, concat. The first three are intersection, union, difference, understood by common sense. concat means directly concatenating two sets, equivalent to SQL's union all. Required parameter; enum type; parameter name must be omitted, parameter value cannot be omitted.
Parameter: **operation_table**
The other table or set with which the current table performs the set operation; its structure must be the same as this table. Required parameter; type is set/identifier/record-table variable; the parameter name must be omitted, the parameter value cannot be omitted.
Parameter: **compare <column_name_for_comparison>**
The field used for comparison. Optional parameter, default is whole-row comparison; type is field identifier or set of field identifiers; parameter name cannot be omitted.
> Perform a whole-row comparison between the OldOrder_example_table and the NewOrder_example_table (this table), find records that are in NewOrder_example_table but not in OldOrder_example_table.
SQLazy: set except; OldOrder_example_table
> Merge the ADeptOrder_example_table (this table) and BDeptOrder_example_table, when the OrderID and Salesperson fields are duplicated, keep the records from ADeptOrder_example_table.
SQLazy: set union; BDeptOrder_example_table; compare OrderID, Salesperson