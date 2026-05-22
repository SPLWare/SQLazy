### Action: join
Description: The right table associates with the left table (this table/current focus table) based on specified reference columns, and the specified columns are joined to the left table, forming a wider table. Supports left join, full join, and join by position (row number).
Syntax: [<reference_column_name>] [with <reference_table_name>] [<join_column_name>] [take {[<original_join_column_name>] [as <new_join_column_name>]}] [option] [condition <condition_expression>]
Parameter: **reference_column_name** 
One or more fields in the left table (current table) used for joining, including # (i.e., the position field/row number field). Required parameter, when the parameter value is omitted, it means the parameter value is the focus column in the context; type is field identifier or set of field identifiers; parameter name must be omitted.
> Employee_table joins with Department_table via the DepartmentID field
Partial NLC code: join DepartmentID…  //ellipsis indicates the code is incomplete and cannot run independently
Parameter: **with <reference_table_name>**
The right table to join to the left table. Required parameter; type is table identifier; parameter name cannot be omitted.
> Partial NLC code: join DepartmentID; with Department_table…
Parameter: **join_column_name**
One or more fields in the right table used for joining, including # (i.e., the position field/row number field). Required parameter, when the parameter value is omitted, it means the parameter value is the primary key of the right table or the column in the right table with the same name as the reference column; type is field identifier or set of field identifiers; parameter name must be omitted.
> Partial NLC code: join DepartmentID; with Department_table; DepartmentID…  //In this example, the join column name is the same as the reference column name, so it can be omitted.
Parameter: **take**
After establishing the join relationship, all columns from the right table that need to be joined to the left table. Required parameter; contains two sub-parameters: **original_join_column_name** and **as <new_join_column_name>**. Using these two parameters once means joining one column from the right table to the left table. If multiple columns need to be joined, they are used multiple times. The parameter name of take cannot be omitted.
Sub-parameter: **original_join_column_name**	A single column in the right table that needs to be joined to the left table. Required parameter; type is a single field identifier; parameter name must be omitted. Note, if the user instruction requires joining all columns, all field names of the right table should be retrieved from the reference table in the context environment and used as the values for this parameter in sequence.
Sub-parameter: **as <new_join_column_name>**  The new column name after renaming the **original_join_column_name**. Optional parameter, meaning renaming is not required; type is a single field identifier; parameter name cannot be omitted.
> Employee_table joins with Department_table via the DepartmentID field, join the DepartmentName and OfficeAddress fields of the Department_table to the left table. Where, the original field name OfficeAddress is renamed to the new field name Address.
NLC: join DepartmentID; with Department_table; DepartmentID；take DepartmentName, OfficeAddress as Address	//This is complete code, no ellipsis.

Parameter: **option**
The join type when joining. Required parameter, when this parameter is absent, it means the parameter value is left_join, as in the previous example; enum type; parameter name must be omitted. There are 4 enum values: left_join, inner/match, full/fill, align.
left_join, inner/match, full/fill: these 3 enum values are understood by common sense, similar to the three join types in SQL.
> Instead of the default left join, use full join to join Employee_table and Department_table
NLC: join DepartmentID; with Department_table; DepartmentID；take DepartmentName, OfficeAddress as Address; full
align: means joining by position or row number #, i.e., records with the same row number are joined together. When using the align method, the reference column name and join column name are both # and can be omitted.
Parameter: **condition <condition_expression>**
This parameter is used to filter the join result after inner join, usually containing fields from the reference table (left table) and the right table, where fields of the reference table must use the @ suffix, and fields of non-reference tables must not have the @ suffix, such as the NLC code snippet: OrderDate>RegistrationDate@, where the field OrderDate is a field of the right table, and the field RegistrationDate is a field of the left table. The function of this parameter is similar to the WHERE condition in SQL's "join on… where…", e.g., SQL code: "select left_table.* from left_table inner join right_table on left_table.id=right_table.id where right_table.OrderDate>left_table.RegistrationDate", the NLC condition is similar to "right_table.OrderDate>left_table.RegistrationDate" in SQL.
Optional parameter; conditional expression; parameter name cannot be omitted. Note, when this parameter is present, only inner join is supported.
> Example: Employee_table joins with Department_table via the DepartmentID field, join the DepartmentManager and DepartmentAddress fields of the Department_table, filter by the condition (EmployeeSalary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains DepartmentName).
NLC: join DepartmentID; with Department_table; DepartmentID; take DepartmentManager DepartmentAddress; inner; condition (EmployeeSalary@>=5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains DepartmentName)