### Action: recurse

Description: In a table with a tree structure, start from some rows and follow the parent-child relationship to find related records level by level.

Syntax: recurse [direction] start <start_condition_expression> take {<expression> [as <new_column_name>]} [level <level_column_name>] [key <key_field>] [parent <parent_column_field>] [condition <condition_expression>]

Note: A "tree structure" here means that the table has two fields: one field uniquely identifies each row (such as EId), and the other field records "which row is the parent of this row" (such as ManagerId). These two fields organize the whole table into a tree; the topmost row has no parent, so it is the root of the tree.

This action is equivalent to a recursive subquery in SQL (WITH RECURSIVE), and generalizes and extends it: the **start** parameter corresponds to the rows selected first in SQL, the **direction**, **parent** and **key** parameters determine how to keep searching level by level afterwards, and the **level** parameter corresponds to the recursion depth.

> All examples in this document use the employee table (focus table) below. In it, EId uniquely identifies each row; ManagerId records "who is the manager of this row"; ReferrerId records "who referred this row"; Michael has neither a manager nor a referrer, so he is the root of both trees.

```
EId	Name	ManagerId	ReferrerId	Dept	Salary
E01	Michael				General Manager Office	30000
E02	Linda	E01	E01	Sales Dept	15000
E03	David	E01	E02	Tech Dept	18000
E04	Ashley	E02	E03	Sales Dept	9000
E05	Rachel	E02	E03	Sales Dept	8000
E06	Emily	E03	E01	Tech Dept	12000
E07	Matthew	E03	E06	Tech Dept	7000
E08	Kevin	E04	E07	Sales Dept	5000
```

Note: By **ManagerId**, the tree is: Michael has Linda and David under him; Linda has Ashley and Rachel under her; David has Emily and Matthew under him; Ashley has Kevin under her. By **ReferrerId**, there is another tree: Michael referred Linda and Emily; Linda referred David; David referred Ashley and Rachel; Emily referred Matthew; Matthew referred Kevin.

#### Parameter description (from basic to advanced)

The first four parameters, **direction**, **start**, **take** and **parent**, are required; writing only these four is enough to use this action. The remaining parameters, **item**, **as**, **level**, **key** and **condition**, are optional; they let you write computed columns, name columns, add a level number column, specify the key, and filter the result.

Parameter: **direction**

Which direction to search. Required parameter; enum type; the parameter name must be omitted, the parameter value cannot be omitted. There are two enum values:

- up: starting from the start row, search for managers; that is, follow the **parent** parameter all the way toward the root.
- down: starting from the start row, search for subordinates; that is, follow the **parent** parameter all the way toward the leaves.

> Example: Starting from Ashley, search upward for all of her managers.

SQLazy: recurse up start (Name="Ashley") take Name parent ManagerId

Result:

```
Name
Ashley
Linda
Michael
```

Explanation: Ashley is the start row (level 1); her manager is Linda (level 2); Linda's manager is Michael (level 3); Michael has no manager, so there is no further level. The order of the rows in the result is exactly the order in which they were found, level by level.

> Example: Starting from Michael, search downward for all of his subordinates.

SQLazy: recurse down start (ManagerId isnull) take Name parent ManagerId

Result:

```
Name
Michael
Linda
David
Ashley
Rachel
Emily
Matthew
Kevin
```

Explanation: Michael is level 1; Linda and David are level 2; Ashley, Rachel, Emily and Matthew are level 3; Kevin is level 4. Records of the same level are placed together, and within the same level they keep the original order of the focus table.

Parameter: **start**

Which rows to start from. Required parameter; type is condition expression; the parameter name cannot be omitted. You write a condition expression, and the rows satisfying it are the start rows; the start rows themselves also appear in the result (counted as level 1).

There can be only one start row, or several; a start row can be the root (the row with no manager), or any row in the middle of the tree.

> Example: Starting from Linda, search downward for her and all of her subordinates.

SQLazy: recurse down start (Name="Linda") take Name parent ManagerId

Result:

```
Name
Linda
Ashley
Rachel
Kevin
```

Explanation: This time the start row is Linda, not the root, so Michael, David, Emily and Matthew do not appear in the result; they are not under Linda.

> Example: The Tech Dept has three people (David, Emily and Matthew); start from all three of them together and search upward for their respective managers.

SQLazy: recurse up start (Dept="Tech Dept") take Name parent ManagerId

Result:

```
Name
David
Emily
Matthew
Michael
```

Explanation: The condition "Dept='Tech Dept'" selects three rows at once (David, Emily and Matthew); these three rows are all start rows, all at level 1. Searching one level further up, their managers are all Michael, so Michael is level 2. When there are several start rows, this action removes duplicate rows automatically, so Michael appears only once.

Parameter: **take**

Which columns the result should contain. Required parameter; type is compound parameter (one parameter made up of several sub-parameters); the parameter name cannot be omitted.

This parameter consists of one or more groups of parameters; each group represents one column in the result and consists of two parameters: one is an expression (its parameter name must be omitted), and the other is the column name of that expression (sub-parameter **as**). When you need several columns, write several groups, separated by commas.

> Example: Starting from Michael, search downward for all records, with the Name and Dept columns in the result.

SQLazy: recurse down start (ManagerId isnull) take Name, Dept parent ManagerId

Result:

```
Name	Dept
Michael	General Manager Office
Linda	Sales Dept
David	Tech Dept
Ashley	Sales Dept
Rachel	Sales Dept
Emily	Tech Dept
Matthew	Tech Dept
Kevin	Sales Dept
```

> Example: The same search, with the Name, Dept and Salary columns in the result.

SQLazy: recurse down start (ManagerId isnull) take Name, Dept, Salary parent ManagerId

Result:

```
Name	Dept	Salary
Michael	General Manager Office	30000
Linda	Sales Dept	15000
David	Tech Dept	18000
Ashley	Sales Dept	9000
Rachel	Sales Dept	8000
Emily	Tech Dept	12000
Matthew	Tech Dept	7000
Kevin	Sales Dept	5000
```

Explanation: The order of the columns in the result is the order in which you write them in "take".

Parameter: **parent**

Tells this action "which column records the manager of this row". Required parameter; type is (column) identifier; the parameter name cannot be omitted, the parameter value cannot be omitted. This action relies on this column to search upward or downward level by level.

Using a different parent column gives a different tree structure, and therefore a different result.

> Example: Along the tree defined by ManagerId, start from Ashley and search upward for all of her managers.

SQLazy: recurse up start (Name="Ashley") take Name, Dept parent ManagerId

Result:

```
Name	Dept
Ashley	Sales Dept
Linda	Sales Dept
Michael	General Manager Office
```

> Example: Switch to the tree defined by ReferrerId and start from Michael, searching downward for all records; the result differs from the previous example.

SQLazy: recurse down start (ReferrerId isnull) take Name, Dept parent ReferrerId

Result:

```
Name	Dept
Michael	General Manager Office
Linda	Sales Dept
Emily	Tech Dept
David	Tech Dept
Matthew	Tech Dept
Ashley	Sales Dept
Rachel	Sales Dept
Kevin	Sales Dept
```

Explanation: By referral, Michael referred Linda and Emily; Linda referred David; David referred Ashley and Rachel; Emily referred Matthew; Matthew referred Kevin. This tree is different from the one seen by ManagerId, so the result is different as well.

Parameter: **item**

Sub-parameter **item** is the expression of each group in the **take** parameter, that is, how the value of this column in the result is calculated. Required parameter; type is expression; the parameter name must be omitted, the parameter value cannot be omitted.

You can write a single column name, such as: Name, Salary; or an expression over one or several columns of the focus table, such as: Salary*0.1, UnitPrice*Quantity*0.1.

Note, this parameter does not support cross-row calculation or aggregation calculation, i.e., the expression cannot contain relative position calculations of the form F[i] or F[a:b], nor aggregation calculations such as sum or average of a set.

> Example: Starting from Ashley, search upward for all of her managers, with only the Salary column in the result.

SQLazy: recurse up start (Name="Ashley") take Salary parent ManagerId

Result:

```
Salary
9000
15000
30000
```

> Example: Starting from Ashley, search upward for all of her managers, and calculate the adjusted salary with the expression "Salary*1.1".

SQLazy: recurse up start (Name="Ashley") take Salary*1.1 parent ManagerId

Result:

```
Salary*1.1
9900
16500
33000
```

Explanation: The expression of this group is not a single column name, so the system uses the expression itself as the column name, which is "Salary*1.1". To give this column a proper name, use the **as** parameter described below.

Parameter: **as**

Sub-parameter **as** is the column name of each group in the **take** parameter, that is, the name that the column calculated by sub-parameter **item** has in the result. Optional parameter; when omitted, the original column name is kept (when the expression is not a single column, the system uses the expression as the column name); type is (column) identifier; the parameter name cannot be omitted, the parameter value cannot be omitted. Note, this parameter must be used together with the **item** of the same group.

> Example: The same search, naming the "Salary*1.1" column above "AdjustedSalary".

SQLazy: recurse up start (Name="Ashley") take Salary*1.1 as AdjustedSalary parent ManagerId

Result:

```
AdjustedSalary
9900
16500
33000
```

> Example: Starting from Michael, search downward for all records, renaming the Name column to "Employee".

SQLazy: recurse down start (ManagerId isnull) take Name as Employee parent ManagerId

Result:

```
Employee
Michael
Linda
David
Ashley
Rachel
Emily
Matthew
Kevin
```

Parameter: **level**

When this parameter is used, the result gains one extra level number column, showing which level each row is on (the start row is level 1). Optional parameter; type is (column) identifier, i.e., the name of the level number column; the parameter name cannot be omitted, the parameter value cannot be omitted.

> Example: Starting from Michael, search downward for all records, and also ask for a level number column named "LevelNo".

SQLazy: recurse down start (ManagerId isnull) take Name level LevelNo parent ManagerId

Result:

```
Name	LevelNo
Michael	1
Linda	2
David	2
Ashley	3
Rachel	3
Emily	3
Matthew	3
Kevin	4
```

> Example: Starting from Ashley, search upward for all of her managers, and also ask for a level number column named "LevelNo".

SQLazy: recurse up start (Name="Ashley") take Name level LevelNo parent ManagerId

Result:

```
Name	LevelNo
Ashley	1
Linda	2
Michael	3
```

Explanation: Whether searching up or down, the start row is always level 1, and each further level adds 1.

Parameter: **key**

The field that uniquely identifies each row; this action uses it to tell whether "this row has already been found". Optional parameter; when omitted, the primary key of the table is used; type is (column) identifier; the parameter name cannot be omitted, the parameter value cannot be omitted.

Note, this parameter is only used internally to recognize rows and does not appear in the result; which columns the result has is determined by the **take** parameter.

> Example: Starting from Michael, search downward for all records, and explicitly write that the key is EId.

SQLazy: recurse down start (ManagerId isnull) take Name level LevelNo key EId parent ManagerId

Result:

```
Name	LevelNo
Michael	1
Linda	2
David	2
Ashley	3
Rachel	3
Emily	3
Matthew	3
Kevin	4
```

> Example: The primary key of the employee table is exactly EId, so this parameter can be omitted, and the result is the same as the previous example.

SQLazy: recurse down start (ManagerId isnull) take Name level LevelNo parent ManagerId

Result:

```
Name	LevelNo
Michael	1
Linda	2
David	2
Ashley	3
Rachel	3
Emily	3
Matthew	3
Kevin	4
```

Parameter: **condition**

Filters the result additionally, keeping only the rows that satisfy this condition. Optional parameter; type is condition expression; the parameter name cannot be omitted.

Note, this parameter only removes the rows that do not satisfy the condition from the result; it does not affect this action's continuing to search the next level.

> Example: Starting from Michael, search downward for all records, and keep only those with a salary of at least 8000 in the result.

SQLazy: recurse down start (ManagerId isnull) take Name, Salary condition (Salary>=8000) parent ManagerId

Result:

```
Name	Salary
Michael	30000
Linda	15000
David	18000
Ashley	9000
Rachel	8000
Emily	12000
```

> Example: Starting from Michael, search downward for all records, and keep only those in the Sales Dept in the result.

SQLazy: recurse down start (ManagerId isnull) take Name, Dept condition (Dept="Sales Dept") parent ManagerId

Result:

```
Name	Dept
Linda	Sales Dept
Ashley	Sales Dept
Rachel	Sales Dept
Kevin	Sales Dept
```

Explanation: Michael is in the General Manager Office, so he is removed by the condition as well; but his subordinates Linda and David are still searched downward as usual, so the records of Linda's branch still appear in the result. This shows that the **condition** parameter only filters the result and does not affect searching further down.
