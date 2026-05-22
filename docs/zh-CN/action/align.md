### Action: align
Syntax: <align_expression> [according <ordered_set_or_other_table>] [take {<original_column_name> [as <new_column_name>]}] [sort_only] [rest_keep] [partition <partition_field>]
Parameter: **align_expression** **according**
These two parameters must be used together. Parameter **align_expression** is an expression related to a field of the focus table, whose result is used for alignment, can be a single column (a type of expression). Required parameter, when no parameter value, it means the default is to use the focus column in the context as the alignment column; type is expression; parameter name must be omitted.
Parameter **according** indicates the reference for alignment, assumed to have no duplicate values, can be an ordered set, or another table. If it's another table, it means using the primary key column values of that table as the reference; if no primary key, using the first column's values as the reference. Required parameter; type is ordered set, table, ordered set identifier, table identifier; parameter name cannot be omitted.
Note: If the **according** parameter contains field values not present in the focus table, the default is to insert a missing record at the corresponding position, with only that field value filled, and other fields empty. Conversely, if the focus table contains field values not in the **according** parameter, those records should be deleted by default. This is an important characteristic of the align action.
> Example: The Employee_table current data is as follows
EId	Dept	Name	Salary
2	HR	Ashley	11000
3	Sales	Rachel	9000
4	HR	Emily	7000
5	Sales	Ashley	16000
6	Marketing	Matthew	11000
Goal: Align the Employee_table by the Dept field according to ["Sales","R&D","HR"].
NLC: align Dept; according ["Sales","R&D","HR"]
Result:
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
	R&D		
2	HR	Ashley	11000
4	HR	Emily	7000
Explanation: The focus table does not have "R&D" present in the ordered set, but has "Marketing" which is not in the ordered set. According to the parameter notes, a new record should be inserted between the records corresponding to Sales and HR, this record only has the Dept value "R&D", other fields empty; records corresponding to "Marketing" should be deleted.

Parameter: **take**
When the **according** parameter is another table, this parameter can be used to join other columns of that table (except the reference column) to the back of the focus table. Optional parameter; composite parameter; parameter name cannot be omitted. This parameter is a composite parameter consisting of one or more pairs of sub-parameters, i.e., sub-parameter **original_column_name** and sub-parameter **as**, each pair representing one column in the other table.
Where, sub-parameter **original_column_name** is a column name in the other table (including the sequence column #) to be joined to the focus table. Required parameter; type is column identifier; parameter name must be omitted.
Where, sub-parameter **as** is the new name after joining the **original_column_name** to the focus table. Optional parameter, default keeps the original name; type is column identifier; parameter name cannot be omitted.
> Align the Dept field of Employee_table (focus table) according to the primary key column (DeptID) of Department_table, join the DepartmentName and Manager fields of the Department_table, where Manager is renamed to ManagerName.
NLC: align Dept; Department_table; take DepartmentName, Manager as ManagerName.
 
Parameter: **sort_only**
If the **according** parameter contains field values not present in the focus table, the default is to insert missing records at the corresponding positions. This parameter is a modification to this default rule, i.e., when this parameter is used, missing records are no longer inserted. Optional parameter; boolean type, parameter name cannot be omitted, parameter value must be omitted.
> Align the Employee_table by Dept field according to ["Sales","R&D","HR"], only sort, do not supplement missing records
NLC: align Dept; according ["Sales","R&D","HR"]; sort_only
Result:
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
2	HR	Ashley	11000
4	HR	Emily	7000
Parameter: **rest_keep**
If the focus table contains field values not in the **according** parameter, the default is to delete those records. This parameter is a modification to this default rule, i.e., when this parameter is used, those records are no longer deleted, but instead are placed at the end of the focus table. Optional parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.
> Align the Employee_table by Dept field according to ["Sales","R&D","HR"], keep the records in Employee_table that cannot be aligned.
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
R&D		
2	HR	Ashley	11000
4	HR	Emily	7000
6	Marketing	Matthew	11000

Parameter: **partition**
Align by partition, partitions do not affect each other, i.e., records in each partition are aligned to the same according parameter separately, conceptually similar to SQL's PARTITION BY. Optional parameter; identifier type; parameter name cannot be omitted.
> Example: Align the records of each customer in Order_example_table by the ProductType field according to the set ["Small Appliances","Textiles","Food"].
NLC: align ProductType; according ["Small Appliances","Textiles","Food"]; partition Customer