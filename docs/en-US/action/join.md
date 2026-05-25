### Action: join
Description: The right table is joined with the left table (current table/focused table) based on the specified key column(s), and the specified columns are appended to the left table to form a wider table. Supports left join, full join, and positional (row number) join.
Syntax: [<at>] [with <with_table>] [<on>] [take {[<original_take_column>] [as <new_take_column>]}] [option] [condition <condition>]
Parameter: **at**
One or more fields in the left table (current table) used for joining, including # (i.e., the position field/row number field). Required parameter; omitting the parameter value means the value is the focused column in the context; type is field identifier or set of field identifiers; parameter name must be omitted.
> Employee table and department table are joined via department number field
Partial NLC code: join dept_no...  // Ellipsis indicates the code is incomplete and cannot run independently

Parameter: **with <with_table>**
The right table to be joined to the left table. Required parameter; type is table identifier; parameter name cannot be omitted.
> Partial NLC code: join dept_no; with dept_table...

Parameter: **on**
One or more fields in the right table used for joining, including # (i.e., the position field/row number field). Required parameter; omitting the parameter value means the value is the primary key of the right table or a column in the right table with the same name as the key column; type is field identifier or set of field identifiers; parameter name must be omitted.
> Partial NLC code: join dept_no; with dept_table; dept_no...  // In this example, the on column has the same name as the at column, so it can be omitted.

Parameter: **take**
All columns in the right table that need to be appended to the left table after the join relationship is established. Required parameter; contains two sub-parameters: **original_take_column**, **as <new_take_column>**. Each use of these two sub-parameters appends one column from the right table to the left table. If multiple columns need to be appended, the parameter must be used multiple times. The parameter name "take" cannot be omitted.
Sub-parameter: **original_take_column** A single column in the right table to be appended to the left table. Required parameter; type is a single field identifier; parameter name must be omitted. Note: If the user instruction requires appending all columns, then all field names of the right table should be extracted from the referenced table in the context and used as the value of this parameter in sequence.
Sub-parameter: **as <new_take_column>** The new column name after renaming the original_take_column. Optional parameter (i.e., renaming is not required); type is a single field identifier; parameter name cannot be omitted.
> Join the employee table and department table via department number field, append the department name and office address fields from the department table to the left table. Rename the original field office address to address.
NLC: join dept_no; with dept_table; dept_no; take dept_name, office_address as address  // This is complete code, no ellipsis.

Parameter: **option**
The join method when establishing the relationship. Required parameter; if this parameter is absent, the default value is left join (as in the previous example); enum type; parameter name must be omitted. Has four enum values: left join, inner, full, align.
left join, inner, full: These three enum values follow common understanding, similar to the three join methods in SQL.
> Instead of the default left join, use full join to join the employee table and department table
NLC: join dept_no; with dept_table; dept_no; take dept_name, office_address as address; full

align: Join by position or row number #, i.e., records with the same row number are joined together. When using align, both the at and on columns are # and can be omitted.
Parameter: **condition <condition>**
This parameter is used to filter the result after the join. It usually contains fields from the base table (left table) and the right table. Fields from the base table must be suffixed with @, while fields from the right table must not have the @ suffix. For example, in the NLC snippet: order_date > registration_date@, the field order_date is from the right table, and registration_date is from the left table. This parameter functions similarly to the WHERE clause in SQL after "join on ... where ...". For example, SQL code: "SELECT left_table.* FROM left_table INNER JOIN right_table ON left_table.id = right_table.id WHERE right_table.order_date > left_table.registration_date". This parameter is analogous to "right_table.order_date > left_table.registration_date" in SQL.
Optional parameter; condition expression; parameter name cannot be omitted. Note: When this parameter is present, only inner and left joins are supported; full join is not supported.
> Example: Join employee table and department table via department number field, append department manager and department address fields from the department table, filter by condition (employee_salary@ >= 5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains dept_name).
NLC: join dept_no; with dept_table; dept_no; take dept_manager dept_address; inner; condition (employee_salary@ >= 5000 and ["Sales Dept 1","Sales Dept 2","Product Dept"] contains dept_name)

Special rule: Convert interval joins into the parameter **condition <condition>**. This rule means that if the user instruction describes a scenario where a field in the left table falls within an interval between two fields in the right table, e.g., "the price@ field in the left table is between budget1 and budget2 in the right table", then this description should be converted into an equivalent interval condition expression "price@ >= budget1 and price@ <= budget2", and that condition expression should be added to the parameter **condition <condition>**. If the open/closed interval is not explicitly described, use a left-closed, right-open interval.
> Example: Join employee table and department table via department number field, append department manager and department address fields from the department table, employee_salary@ is between salary_lower and salary_upper, filter by condition (["Sales Dept 1","Sales Dept 2","Product Dept"] contains dept_name).
NLC: join dept_no; with dept_table; dept_no; take dept_manager dept_address; inner; condition ((employee_salary@ >= salary_lower and employee_salary <= salary_upper) and (["Sales Dept 1","Sales Dept 2","Product Dept"] contains dept_name))