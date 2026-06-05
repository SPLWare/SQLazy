#### function: code_find
Syntax: (<object_parameter> code_find <code_value>)
Return: data. Search the first field of the table <object_parameter> using <code_value>. If a matching record is found, return the value of the second field. If no match, return null.
Parameter **<object_parameter>**: a code table with key-value or code-value structure.
Parameter **<code_value>**: the key value to search for. Required parameter; simple data type; parameter name omitted.
> Example: The employee table has several fields, the first two are employee_id and employee_name. Now use 1001 to perform a code lookup on the employee table.
NLC snippet: (employee_table code_find 1001) // returns an employee name.
