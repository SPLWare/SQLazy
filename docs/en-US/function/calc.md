#### function: calc
Syntax: (<table> calc <expression> [<aggregation>] [<condition_expression>])
Return: Computes <expression> over <table>; when <aggregation> is given, the records satisfying the condition are aggregated, otherwise the <expression> value of the first record is returned. When compiled to SQL it becomes a select ... from table where ... subquery.
Parameter **<table>**: the table to compute on. Required parameter; type is table; parameter name omitted.
Parameter **<expression>**: the value to compute. Required parameter; type is expression; the parameter name must be omitted. For dimension fields the foreign-key attribute form is supported, i.e. use . to reference a field of the foreign-key table, multiple levels allowed.
Parameter **<aggregation>**: when omitted, the <expression> value of the first record is taken; when given, the records satisfying the condition are aggregated. Optional parameter; enumerated type; the parameter name must be omitted, the enum value cannot be omitted. Enum values: sum, max, min, count, icount, avg.
Parameter **<condition_expression>**: condition expression used to keep the records that satisfy it. Optional parameter; type is condition expression; the parameter name must be omitted. Inside the condition, an identifier with the suffix @ refers to a field of the outer table; omitting it means no condition.
> Example: Compute the highest salary in the employee table.
SQLazy snippet: (employee_table calc salary, max) // returns the highest salary
> Example: Compute the average salary of the Tech Dept.
SQLazy snippet: (employee_table calc salary, avg; (dept = "Tech Dept")) // returns the average salary of the Tech Dept
> Example: Take the department name of the department that the first employee record belongs to (foreign-key attribute form).
SQLazy snippet: (employee_table calc department_table.department_name) // returns the name of the department of the first record
