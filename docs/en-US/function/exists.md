#### function: exists
Syntax: (<table> exists <condition_expression>)
Return: boolean. Returns true if <table> has a record satisfying <condition_expression>, otherwise false. When compiled to SQL it becomes an exists (select 1 from ... where ...) subquery.
Parameter **<table>**: the table to be checked. Required parameter; type is table; parameter name omitted.
Parameter **<condition_expression>**: the condition that records in the table must satisfy. Required parameter; type is condition expression; the parameter name must be omitted. Inside the condition, an identifier with the suffix @ refers to a field of the outer table (the table outside this function).
> Example: Check whether the department table has "Sales Dept".
SQLazy snippet: (department_table exists (department_name = "Sales Dept")) // returns true
> Example: For each row of the employee table, check whether the department table has a department with the same department number.
SQLazy snippet: (department_table exists (department_number = department_number@)) // returns true if present, otherwise false
