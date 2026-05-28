### Action: match
Syntax: [<base_column_name>] [with <associated_table_name>] [<join_column_name>] [option] [condition <condition_expression>]
Parameter: **base_column_name** 
One or more fields in the left table (current table) used for matching, including # (the positional field/row number field). Optional parameter; if absent, it means that a fully non-equi match must be established via the **condition <condition_expression>** parameter; if present, an equi-match is established using this parameter, and a non-equi match condition can be further added using **condition <condition_expression>**. Type is a field identifier or a set of field identifiers; the parameter name must be omitted.
> Match the employee table with the department table on the department number field, and delete records in the employee table that have no match.
Partial NLC code: match department_number…  // the ellipsis indicates the code is incomplete and not yet runnable
Parameter: **with <associated_table_name>**
The right table to be matched against the left table. Required parameter; type is a table identifier; the parameter name cannot be omitted.
> Perform an equi-match between the employee table and department table on the department number field, and filter out the records in the employee table that have a match.
Partial NLC code: match department_number; with department_table…	// This example and the previous one are two ways of expressing the same calculation.
Parameter: **join_column_name**
One or more fields in the right table used for matching, including # (the positional field/row number field). Optional parameter; omitting the parameter value means the value is the primary key or a column in the right table with the same name as the base column; type is a field identifier or a set of field identifiers; the parameter name must be omitted.
> Through the department number field, filter the employee table to keep records that can be matched with the department table.
Partial NLC code: match department_number; with department_table; department_number…  // Here the join column name is the same as the base column name, so it can be omitted. This example and the two above are different expressions of the same calculation.
Parameter: **option**
The filtering method after matching, with two parameter values (enumeration values): exists, miss. The former means keeping records that have a match, the latter means keeping records that do not have a match.
Optional parameter; if absent, the default parameter value is "exists", as in the previous example; enumerated type; the parameter name must be omitted.
> Match the employee table with the department table on the department number field, and keep records in the employee table that have no match.
NLC: match department_number; with department_table; department_number; miss
Parameter: **condition <condition_expression>**
This parameter is the condition expression for the match. It usually contains fields from the base table (left table) and the right table, where base table fields must have an @ suffix and non-base table fields must have no @ suffix, e.g., in NLC code: order_date>registration_date@, here order_date is a right table field and registration_date is a left table field. The role of this parameter is similar to the "where" condition in "join on… where…" in SQL, e.g., in SQL code: "select left_table.* from left_table join right_table on left_table.id=right_table.id where right_table.order_date>left_table.registration_date", the NLC condition expression is analogous to "right_table.order_date>left_table.registration_date" in SQL.
Optional parameter; if absent, the **base_column_name** parameter must be present; if present, the **base_column_name** parameter may be absent. Condition expression; the parameter name cannot be omitted.
> Match the employee table and department table on the department number field, filter with the condition (employee_salary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name), and keep records in the employee table that have no match.
NLC: match department_number; with department_table; department_number; miss; condition (employee_salary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)

> Match the employee table and department table, filter with the condition (employee_salary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name), and keep records in the employee table that have no match.
NLC: match with department_table; miss; condition (employee_salary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)
