### Action: match
Syntax: [<reference_column_name>] [with <reference_table_name>] [<join_column_name>] [option] [condition <condition_expression>]
Parameter: **reference_column_name** 
One or more fields in the left table (current table) used for matching, including # (i.e., the position field/row number field). Optional parameter, when the parameter value is omitted, it means the parameter value is the focus column in the context; type is field identifier or set of field identifiers; parameter name must be omitted.
> Employee_table and Department_table match via the DepartmentID field, delete records in Employee_table that cannot be matched
Partial NLC code: match DepartmentID…  //ellipsis indicates the code is incomplete and cannot run independently
Parameter: **with <reference_table_name>**
The right table used to match against the left table. Required parameter; type is table identifier; parameter name cannot be omitted.
> Employee_table and Department_table perform an equality match via the DepartmentID field, filter out records in Employee_table that can be matched
Partial NLC code: match DepartmentID; with Department_table…	//This example and the above example are different expressions of the same calculation.
Parameter: **join_column_name**
One or more fields in the right table used for matching, including # (i.e., the position field/row number field). Optional parameter, when the parameter value is omitted, it means the parameter value is the primary key or the column in the right table with the same name as the reference column; type is field identifier or set of field identifiers; parameter name must be omitted.
> Filter out records from Employee_table that can be matched with Department_table via the DepartmentID field.
Partial NLC code: match DepartmentID; with Department_table; DepartmentID…  //In this example, the join column name is the same as the reference column name, so it can be omitted. This example and the previous two examples are different expressions of the same calculation.
Parameter: **option**
The filter method after matching, with two parameter values (enum values): miss/not_exists, exists. The former means keeping records that can be matched, the latter means keeping records that cannot be matched.
Optional parameter, when this parameter is absent, it means the default parameter value is "miss/not_exists", as in the previous example; enum type; parameter name must be omitted.
> Employee_table and Department_table match via the DepartmentID field, keep records in Employee_table that cannot be matched
NLC: match DepartmentID; with Department_table; DepartmentID; exists
Parameter: **condition <condition_expression>**
This parameter is a condition for matching, usually containing fields from the reference table (left table) and the right table, where fields of the reference table must use the @ suffix, and fields of non-reference tables must not have the @ suffix, e.g., NLC code: OrderDate>RegistrationDate@, where the field OrderDate is a field of the right table, and the field RegistrationDate is a field of the left table. The function of this parameter is similar to the WHERE condition in SQL's "join on… where…", e.g., SQL code: "select left_table.* from left_table join right_table on left_table.id=right_table.id where right_table.OrderDate>left_table.RegistrationDate", the NLC condition is similar to "right_table.OrderDate>left_table.RegistrationDate" in SQL.
Optional parameter; conditional expression; parameter name cannot be omitted.
> Employee_table and Department_table match via the DepartmentID field, filter by the condition (EmployeeSalary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains DepartmentName), keep records in Employee_table that cannot be matched.
NLC: match DepartmentID; with Department_table; DepartmentID; exists; condition (EmployeeSalary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains DepartmentName)