### Action: expand
Syntax: [<list>] [take {<original_column_name> [as <new_column_name>]}]
Parameter: **list** 
One of several forms used to generate a set, including: constant set (including identifiers), natural number N, expressions that can generate a set, other tables in the context (identifiers). Required parameter; type is multiple forms; parameter name must be omitted.
First form: When the list parameter is a constant set, assuming the number of set members is M, each record of the focus table is expanded into M records, with the set members filled into the focus column in sequence.
> Student_table (focus table) originally has 2 records, the focus column is Subject, data as follows:
StudentName	Class	Subject
Zhang San	5-2		
Li Si	5-2
Requirement: Now expand each record into 3 records using the ordered set ["Physics","Math","English"].
Expected result:
StudentName	Class	Subject
Zhang San	5-2		Physics
Zhang San	5-2		Math
Zhang San	5-2		English
Li Si	5-2		Physics
Li Si	5-2		Math
Li Si	5-2		English
NLC: expand ["Physics","Math","English"]	
Second form: When the list parameter is an integer N, the set is the natural numbers 1 to N. Each record of the focus table is expanded into N records, with the set members filled into the focus column in sequence.
> Student_table originally has 2 records, the focus column is Semester, data as follows:
StudentName	Class	Semester
Zhang San	5-2		
Li Si	5-2
Requirement: Now expand each record into 2 records using the natural number 2.
Expected result:
StudentName	Class	Semester
Zhang San	5-2		1
Zhang San	5-2		2
Li Si	5-2		1
Li Si	5-2		2
NLC: expand 2

Third form: When the parameter list is an expression that can generate a set, assuming the set has M members, each record of the focus table expands into M rows, and the set members are sequentially filled into the focus column. Note that this expression is restricted; the generated set is relatively fixed and cannot vary with the current record of the focus table.  
> The student table (focus table) originally has 2 records, the focus column is Subject, data as follows:  
Student Name	Class	Subject  
Zhang San	Class 5 Grade 2	  
Li Si	Class 5 Grade 2  
Requirement: Now use the expression (if(parameter1>2 then ["Chemistry","Language"]; else ["Physics","Math","English"])) to expand each record into multiple rows, assuming variable parameter1=3.  
Expected result:  
Student Name	Class	Subject  
Zhang San	Class 5 Grade 2	Chemistry  
Zhang San	Class 5 Grade 2	Language  
Li Si	Class 5 Grade 2	Chemistry  
Li Si	Class 5 Grade 2	Language  
NLC: expand (if(parameter1>2 then ["Chemistry","Language"]; else ["Physics","Math","English"]))  

Fourth form: When the list parameter is another table in the context, if that table has a primary key, the set is all values of the primary key column; if it has no primary key, the set is all values of the first column of that table. Assuming the set length is M, each record of the focus table is expanded into M records, with the set members filled into the focus column in sequence.
> In the context there is a Gift_table, with structure [GiftName, Brand, Grade], no primary key, with N records. The focus table is Customer_table, the focus column is PlannedGift. Now require to sequentially write the GiftName of Gift_table into the PlannedGift column of Customer_table, expanding each record into N records.
NLC: expand	Gift_table

Parameter: **take**
When the **list** parameter is another table, this parameter can be used to join other columns of that table (except the column that generates the set) to the back of the focus table. Optional parameter; composite parameter; parameter name cannot be omitted. This parameter is a composite parameter consisting of one or more pairs of sub-parameters, i.e., sub-parameter **original_column_name** and sub-parameter **as**, each pair representing one column in the other table.
Where, sub-parameter **original_column_name** is a column name in the other table (including the sequence column #) to be joined to the focus table. Required parameter; type is column identifier; parameter name must be omitted.
Where, sub-parameter **as** is the new name after joining the **original_column_name** to the focus table. Optional parameter, default keeps the original name; type is column identifier; parameter name cannot be omitted.
> Write the GiftName of Gift_table sequentially into the PlannedGift column (focus column) of Customer_table (focus table), and join the Brand and Grade fields to Customer_table, expanding each record into N records, where the field Grade is renamed to Level.
NLC: expand	Gift_table; take Brand, Grade as Level
