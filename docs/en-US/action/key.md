### Action: key
Syntax: [<key>] [time] [index]
Parameter: [<key>] 
Set one or more columns as the primary key. Required parameter, when omitted, it means using the focus column in the context as the primary key; field or set of fields type; parameter name must be omitted.
> Example: set the primary key on Order_example_table.
NLC: key OrderID
Parameter: **time**
Indicates that the last column in the parameter [<key>] is a time key. Optional parameter; boolean type (parameter name cannot be omitted, parameter value must be omitted).
> Example: The Employee_table has fields [EID,NAME,DEPT,SALARY,HIREDATE]. The primary key [EID,DEPT,HIREDATE] needs to be set, where HIREDATE is a time key.
NLC:
key EID,DEPT,HIREDATE; time
Parameter: **index**
Indicates building an index while creating the primary key, which can greatly improve query performance for such primary key queries in the future. Optional parameter; boolean type (parameter name cannot be omitted, parameter value must be omitted).
> Example: set the primary key [EID,DEPT,HIREDATE] on the Employee_table, where HIREDATE is a time key, and build an index at the same time.
key EID,DEPT,HIREDATE; time; index