### Action: pivot

#### Mode Determination (Highest Priority, Must Execute First)

Pivot has two mutually exclusive modes: 1. row_to_column (default mode) 2. column_to_row (inverse mode)

Determination Rules (Must Execute Strictly):

If the natural language contains: "row to column / turn to multiple columns / generate new columns / columns as headers / wide table"

→ Must use [row_to_column], prohibit using "inverse"

If the natural language contains: "column to row / turn to multiple rows / expand to rows / unpivot / inverse operation / long table"

→ Must use [inverse]

If both appear or semantics conflict:

→ Output error, guessing is not allowed

If no keywords are explicitly present:

→ Default to [row_to_column], prohibit adding "inverse"

#### Quick Discrimination

Row_to_column (Pivot):

- Data becomes "wider"
- Rows decrease, columns increase
- Typical: two columns (title column + value column) → become multiple columns (each value of the original title column becomes a new column name, and the values of the original value column become the values of the new columns)

Column_to_row (Unpivot):

- Data becomes "longer"
- Columns decrease, rows increase
- Typical: multiple columns → become two columns (the titles of the original multiple columns become the values of a new title column, and the values of the original multiple columns become the values of a new value column)

Syntax: [group {[<group_column>] [as <new_column_name>]}] [name <column_name>] [value <column_name>] [option] [header <title_value_set>]

Parameter: **group**

This parameter is a composite parameter, consisting of one or more sets of sub-parameters, each set composed of **group_column**, **as <new_column_name>**, representing a group column. Multiple group columns require multiple sets of sub-parameters. Required parameter; type is composite parameter; parameter name cannot be omitted

Parameter: **group_column**

The single group column to remain fixed during pivot. Required parameter; type is (column) identifier; parameter name must be omitted.

Parameter: **as**

The new column name for the **group_column** after pivot. Optional parameter, default keeps the original column name unchanged; type is (column) identifier; parameter name cannot be omitted. Note, this parameter must be paired with the **group_column** parameter.

> The fields of the branch department budget table are Branch, Dept, State, Amount. Data as follows:

```
Branch	Dept	State	Amount
branch1	Administration	Florida	28000
branch1	Finance	California	6000
branch2	Finance	Florida	20000
branch2	Finance	New York	11000
branch3	Finance	Texas	11000
branch2	HR	Texas	10000
branch1	Marketing	California	31000
branch1	Marketing	New York	12000
branch2	Marketing	Pennsylvania	10000
branch3	Marketing	Texas	4000
branch1	Production	Florida	15000
branch3	Production	Pennsylvania	10000
branch3	R&D	Pennsylvania	24000
branch2	R&D	Texas	32000
branch1	Sales	California	28000
branch1	Sales	Florida	4000
branch2	Sales	New York	15000
branch3	Sales	Texas	12000
```

Requirement: Keep Branch, Dept unchanged, perform **row_to_column** on the other columns/group data, with the original State values as the titles of the new columns, and the original Amount values as the values of the new columns, rename Branch to Company.

Expected result:

```
Company	Dept	Florida	California	New York	Texas	Pennsylvania
branch1	Administration	28000				
branch1	Finance		6000			
branch1	Marketing		31000	12000		
branch1	Production	15000				
branch1	Sales	4000	28000			
branch2	Finance	20000		11000		
branch2	HR				10000	
branch2	Marketing					10000
branch2	R&D				32000	
branch2	Sales			15000		
branch3	Finance				11000	
branch3	Marketing				4000	
branch3	Production					10000
branch3	R&D					24000
branch3	Sales				12000	
```

SQLazy: pivot group Branch as Company, Dept; name State; value Amount

Parameter: **name <column_name>**

After pivot, the values of the specified single column in the original table will become the titles of the new columns; this single column is the title column. Required parameter; type is (column) identifier; parameter name cannot be omitted.

> For example, the earlier partial SQLazy code: name State

Parameter: **value <column_name>**

After pivot, the values of the specified single column in the original table will become the values of the new columns; this single column is the value column. Required parameter; type is (column) identifier; parameter name cannot be omitted.

> In the above SQLazy code: value Amount

Parameter: **option**

Only 6 calculation methods have writable enum values: sum, count, avg, max, min, inverse. Among them inverse is the only value that switches the operation mode, the others are aggregation algorithms under row_to_column. In addition there is 1 calculation method that cannot be written, i.e. the default of taking the 1st record within the group (this prompt refers to it as "first_record", which is not a SQLazy keyword and must never appear in the output). Optional parameter, default parameter value is "first_record" (generally a default value may be written or not, but the default "first_record" is special, it has no corresponding enum value, so this parameter must be omitted); enum type; parameter name must be omitted. Note: the enum values "first" and "last" of summarize and compute do not apply to pivot and must not be borrowed; when the user says "take the first/1st record", always omit this parameter.

sum, count, avg, max, min, and the default "first_record": this is a set of aggregation algorithms, i.e., after row_to_column pivot, when a row of the title column corresponds to multiple values in the value column, a single value can be obtained through aggregation. This situation generally occurs because there are other unused columns in the original table besides the group column, title column, and value column.

> For the earlier branch department budget table, group by Branch, perform row_to_column on the group data, State as the title column, Amount as the value column, take the maximum of Amount.

Expected result:

```
Branch	Florida	California	New York	Texas	Pennsylvania
branch1	28000	31000	12000		
branch2	20000		15000	32000	10000
branch3				12000	24000
```

SQLazy: pivot group Branch; name State; value Amount; max

Explanation: Because the Dept column is not used, after pivot each value column corresponds to multiple Dept values, which can be aggregated into a single value.

Note, the default "first_record" is also an aggregation operation, meaning taking the 1st one from multiple values; it has no writable enum value and can only be obtained by omitting the parameter **option**.

> For the earlier branch department budget table, group by Branch, perform row_to_column on the group data, State as the title column, Amount as the value column, take the first value.

Expected result:

```
Branch	Florida	California	New York	Texas	Pennsylvania
branch1	28000	6000	12000		
branch2	20000		11000	10000	10000
branch3				11000	10000
```

SQLazy: pivot group Branch; name State; value Amount

Explanation: "first_record" is only this prompt's name for the default calculation method, it is not a keyword and cannot be written, so this parameter can only be omitted, which is what the code above does.

Wrong form (forbidden output): SQLazy: pivot group Branch; name State; value Amount; first_record

> For the earlier branch department budget table, group by Branch, perform row_to_column on the group data, State as the title column, Amount as the value column, count Amount.

SQLazy: pivot group Branch; name State; value Amount; count

Explanation: Each cell counts the number of records for that Branch and State (same aggregation branch as sum), to contrast with sum.

inverse: The other calculation methods are all aggregation methods under the large category of **row_to_column**. This calculation method is not an aggregation method, but another operation mode (special operation mode/mode switch), it is **column_to_row**, the inverse operation of **row_to_column**. Specifically, it keeps the group columns unchanged, and performs **column_to_row** on the group data/other columns, where the titles of the other columns of the original table become the values of the new title column, and the column values of the other columns of the original table become the values of the new value column.

> Below is the wide budget table, where Company and Dept are group columns.

```
Company	Dept	Florida	California	New York	Texas	Pennsylvania
branch1	Administration	28000				
branch1	Finance		6000			
branch1	Marketing		31000	12000		
branch1	Production	15000				
branch1	Sales	4000	28000			
branch2	Finance	20000		11000		
branch2	HR				10000	
branch2	Marketing					10000
branch2	R&D				32000	
branch2	Sales			15000		
branch3	Finance				11000	
branch3	Marketing				4000	
branch3	Production					10000
branch3	R&D					24000
branch3	Sales				12000	
```

Requirement: Keep Company (renamed to Branch), Dept unchanged, perform column_to_row on the other columns, the titles of the original other columns become the values of the new column State, and the column values of the original other columns become the values of the new column Amount.

Expected result:

```
Branch	Dept	State	Amount
branch1	Administration	Florida	28000
branch1	Finance	California	6000
branch2	Finance	Florida	20000
branch2	Finance	New York	11000
branch3	Finance	Texas	11000
branch2	HR	Texas	10000
branch1	Marketing	California	31000
branch1	Marketing	New York	12000
branch2	Marketing	Pennsylvania	10000
branch3	Marketing	Texas	4000
branch1	Production	Florida	15000
branch3	Production	Pennsylvania	10000
branch3	R&D	Pennsylvania	24000
branch2	R&D	Texas	32000
branch1	Sales	California	28000
branch1	Sales	Florida	4000
branch2	Sales	New York	15000
branch3	Sales	Texas	12000
```

SQLazy: pivot group Company as Branch, Dept; name State; value Amount; inverse

Parameter: **header <title_value_set>**

After pivot, the titles of the new columns are by default produced from the column values of the parameter **name <column_name>**. This parameter can fix the title values that participate in the pivot: each listed value generates one column and the columns follow the written order; a listed value that does not exist in the current data still generates a column (that column is empty); an unlisted title value does not participate in the pivot and generates no column. This parameter only decides which columns are generated, it does not filter out records: even if a group has no data under the retained title values, that row still appears (all its columns are empty). Optional parameter; type is field set/identifier set; parameter name cannot be omitted.

> For the branch department budget table, group by Branch, Dept, perform row_to_column on the group data, State as the title column, Amount as the value column, fix the title values that participate in the pivot as Florida, California, New York, Texas, with Pennsylvania not participating in the pivot.

SQLazy: pivot group Branch, Dept; name State; value Amount; header Florida,California,New York,Texas

Result:

```
Branch	Dept	Florida	California	New York	Texas
branch1	Administration	28000			
branch1	Finance		6000		
branch1	Marketing		31000	12000	
branch1	Production	15000			
branch1	Sales	4000	28000		
branch2	Finance	20000		11000	
branch2	HR				10000
branch2	Marketing				
branch2	R&D				32000
branch2	Sales			15000	
branch3	Finance				11000
branch3	Marketing				4000
branch3	Production				
branch3	R&D				
branch3	Sales				12000
```
