### Action: recurse

Description: In a table with a tree structure, start from some rows and follow the parent-child relationship to find related records level by level.

Syntax: recurse [direction] start <start_condition_expression> take {<expression> [xitem <recursive_expression>] [as <new_column_name>]} [level <level_column_name>] [key <key_field>] [parent <parent_column_field>] [condition <condition_expression>]

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

The first three parameters, **direction**, **start** and **take**, are required; writing only these three is enough to use this action. At least one of **parent** and **condition** must be written: when **parent** is given, this action follows the parent-child relationship level by level; when **parent** is omitted, **condition** must describe by itself how to search level by level. The remaining parameters, **item**, **as**, **xitem**, **level** and **key**, are optional; they let you write computed columns, name columns, carry the previous level's result to the next level, add a level number column and specify the key.

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

There can be only one start row, or several; a start row can be the root (the row with no manager), or any row in the middle of the tree. When there are several start rows, they are processed one by one in the original order of the focus table; each start row brings out the rows it finds, and duplicate rows are kept (when searching upward, each start row brings out its whole chain up to the root, see the example below).

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
Michael
Emily
David
Michael
Matthew
David
Michael
```

Explanation: The condition "Dept='Tech Dept'" selects three rows at once (David, Emily and Matthew); these three rows are all start rows, all at level 1. When there are several start rows, each start row in turn brings out its whole chain up to the root: David's manager is Michael; Emily's manager is David, and David's manager is Michael; Matthew's manager is David, and David's manager is Michael. This action no longer removes duplicate rows, so David appears 3 times (once as a start row, twice as a manager) and Michael appears 3 times (all as a manager).

Parameter: **take**

Which columns the result should contain. Required parameter; type is compound parameter (one parameter made up of several sub-parameters); the parameter name cannot be omitted.

This parameter consists of one or more groups of parameters; each group represents one column in the result and consists of two parameters: one is an expression (its parameter name must be omitted), and the other is the column name of that expression (sub-parameter **as**). When the previous level's result has to be carried to the next level, a group may additionally have a recursive expression (sub-parameter **xitem**). When you need several columns, write several groups, separated by commas.

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

Tells this action "which column records the manager of this row". Optional parameter, but at least one of this parameter and **condition** must be written; type is (column) identifier; the parameter name cannot be omitted, the parameter value cannot be omitted. This action relies on this column to search upward or downward level by level.

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

Parameter: **xitem**

Sub-parameter **xitem** is the "recursive expression" of each group in the **take** parameter; it computes this column's value on the following levels. Optional parameter; when omitted, the **item** of the same group is used; type is expression; the parameter name cannot be omitted, the parameter value cannot be omitted.

The **item** described above computes the value of the first level (the start rows); once **xitem** is given, this column's value from the second level on is computed by **xitem**. Its purpose is to carry the previous level's result to the next level: in a recursive expression, an identifier followed by @ means "the value of this column in the previous level's result", while an identifier without @ means "the column of the current row in the source table". So cumulative sums, paths and inherited parent attributes (anything that needs the previous level's result) have to use this parameter.

Note, this parameter likewise does not support relative position calculations of the form F[i] or F[a:b], nor aggregations such as sum or average of a set.

> Example: Starting from Michael, search downward for all records and also compute each person's cumulative salary (their own salary plus the salaries of all managers above them).

SQLazy: recurse down start (ManagerId isnull) take Name, Salary xitem (ifn(CumulativeSalary@, 0) + Salary) as CumulativeSalary parent ManagerId

Result:

```
Name	CumulativeSalary
Michael	30000
Linda	45000
David	48000
Ashley	54000
Rachel	53000
Emily	60000
Matthew	55000
Kevin	59000
```

Explanation: Michael is the start row, so **item** "Salary" gives 30000; on each following level the value is computed by **xitem**: `CumulativeSalary@` takes the cumulative salary of the previous level (that is, of the manager) and adds this row's salary, so Linda=30000+15000=45000, David=30000+18000=48000, Ashley=45000+9000=54000. The 0 in `ifn(..., 0)` is only a fallback; in a normal recursion the previous level always has a value.

> Example: Starting from Michael, search downward for all records and compute, for each person, the name path from the root to that person.

SQLazy: recurse down start (ManagerId isnull) take (concat(Name, "/")) xitem (concat(ifn(Path@, ""), Name, "/")) as Path parent ManagerId

Result:

```
Path
Michael/
Michael/Linda/
Michael/David/
Michael/Linda/Ashley/
Michael/Linda/Rachel/
Michael/David/Emily/
Michael/David/Matthew/
Michael/Linda/Ashley/Kevin/
```

Explanation: The start row uses **item** and gives "Michael/"; each following level uses **xitem**, where `Path@` is the path of the previous level, then appends this row's name and a slash, so Ashley's row is "Michael/Linda/Ashley/".

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

Recursion condition. Optional parameter; type is condition expression; the parameter name cannot be omitted, the parameter value cannot be omitted.

Note, this parameter works together with the **parent** parameter: when **parent** is given, this parameter is an additional condition; when **parent** is omitted, this parameter itself serves as the recursion condition. In the condition, an identifier followed by @ takes the value from the previous round's take table, while an identifier without @ takes the column of the current round's source table (that is, the base table).

Note, when **parent** and this parameter are used together, this parameter means "stop at this row": if a row found at some level does not satisfy the condition, that row does not appear in the result, and the search does not continue from it, so the whole chain beyond it is lost. In the first example below, Ashley (9000) at level 2 satisfies the condition, so the search goes on upward; Linda (15000) at level 3 does not satisfy it, so the chain ends there and Michael further up does not appear either.

> Example: Starting from Kevin, search upward, but keep only the rows whose salary is below 10000.

SQLazy: recurse up start (Name="Kevin") take Name level LevelNo parent ManagerId condition (Salary < 10000)

Result:

```
Name	LevelNo
Kevin	1
Ashley	2
```

Explanation: This time **parent** is given, so **condition** is only an additional condition, and only the rows satisfying it stay in the result. Kevin (5000) and Ashley (9000) satisfy it; Linda (15000) further up does not, so the chain ends there and Michael (30000) does not appear either. The level number is still counted by levels and is not affected by the condition.

> Example: Without **parent**, let **condition** itself describe how to search level by level.

SQLazy: recurse down start (ManagerId isnull) take EId, Name condition (ManagerId = EId@)

Result:

```
EId	Name
E01	Michael
E02	Linda
E03	David
E04	Ashley
E05	Rachel
E06	Emily
E07	Matthew
E08	Kevin
```

Explanation: This time **parent** is omitted, so **condition** itself is the recursion condition. The first round finds Michael (E01), so this round searches with "ManagerId = E01" and finds Linda (E02) and David (E03); the next round searches with "ManagerId = E02" and "ManagerId = E03" respectively and finds Ashley, Rachel, Emily and Matthew; the last round searches with "ManagerId = E04" and finds Kevin. An identifier followed by @ takes the value from the previous round's take table; this example writes EId into **take**, so EId@ is the EId of the row found in the previous round. The result of this example is exactly the same as writing "parent ManagerId".
