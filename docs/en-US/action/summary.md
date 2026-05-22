### Action: summary
#### Mode Determination (Highest Priority, Must Execute First)
Summary has two mutually exclusive aggregation modes: 1. single aggregation 2. double aggregation
Single aggregation means: a summary column (a set of summary parameters) executes only one value aggregation algorithm, i.e., an aggregation algorithm that reduces multiple records to a single value, specifically: first, last, sum, avg, count, icount, variance, std_deviation, max, min. Syntax: [summary {[condition <filter_condition>] [<aggregated_expression>] [value_aggregation_algorithm] [delimeter <symbol>] [as <new_column_name>]}] [group {[<group_expression>] [as <new_column_name>]}]
Double aggregation means: a summary column (a set of summary parameters) needs to perform two aggregations. The first is a record aggregation algorithm, which reduces multiple records to one record or multiple records with the same value, specifically: argmax, argmin. For example, the record with the maximum order amount; when the maximum value is not repeated, the result is one record; when the maximum value is repeated, the result is multiple records. After the first aggregation algorithm, the second value aggregation algorithm (the aggregation algorithm that reduces multiple records to a single value) must be immediately executed to get the final result. For instance, the first aggregation finds multiple records with the maximum order amount, and the second aggregation calculates the sum of amounts from these records; the complete algorithm is to find the sum of amounts of the records with the maximum order amount. NLC's double aggregation is similar to SQL's KEEP function. Syntax: [summary {[condition <filter_condition>] [<aggregated_expression>] [record_aggregation_algorithm] [<aggregated_expression>] [value_aggregation_algorithm] [delimeter <symbol>] [as <new_column_name>]}] [group {[<group_expression>] [as <new_column_name>]}]
Determination Rules (Must Execute Strictly):
If the aggregation calculation expressed by the natural language includes: argmax, argmin
→ must use double aggregation.
Conversely, if the aggregation calculation expressed by the natural language does not include: argmax, argmin, but only: first, last, sum, avg, count, icount, variance, std_deviation, max, min.
→ must use single aggregation
If both appear or semantics conflict:
→ output error, guessing is not allowed
If argmax or argmin is not explicitly expressed:
→ default to single aggregation.
#### Parameter Structure Description
The parameters of this action are composed of two parts: summary and group. The summary part's parameter name is "summary", must be omitted. It consists of one or more sets of identically structured parameters, each set representing a summary column, composed of 5 (single aggregation) or 7 parameters (double aggregation). For single aggregation, one set of parameters is: [condition <filter_condition>] [<aggregated_expression>] [value_aggregation_algorithm] [delimeter <symbol>] [as <new_column_name>]; for double aggregation, one set of parameters is: [condition <filter_condition>] [<aggregated_expression>] [record_aggregation_algorithm] [<aggregated_expression>] [value_aggregation_algorithm] [delimeter <symbol>] [as <new_column_name>]. The group part's parameter name is "group", cannot be omitted, and also consists of one or more sets of identically structured parameters, each set representing a group column, composed of 2 parameters: [<group_expression>] [as <new_column_name>]. First, explain the summary part parameters.
#### Parameter Descriptions
Parameter: **aggregated_expression**
The expression targeted when performing aggregation calculation on data within a group, usually an expression related to the original column, including a single column (a type of expression). For example: UnitPrice*Quantity, an expression containing multiple columns; Amount, a single column. Required parameter; type is expression; parameter name must be omitted, parameter value cannot be omitted. Note, this parameter must be used together with the **value_aggregation_algorithm** or **record_aggregation_algorithm**. A certain summary column has only one **aggregated_expression** parameter for single aggregation, and definitely two **aggregated_expression** parameters for double aggregation. Note, this parameter does not support cross-row calculation and aggregation calculation, i.e., the expression cannot contain relative position calculations like F[i], F[a:b], nor aggregate calculations like sum, average of a set.
Parameter: **value_aggregation_algorithm**
A fixed algorithm that aggregates data within a group to produce a single numeric value. Required parameter; enum type; parameter name must be omitted, parameter value cannot be omitted. This parameter must be used together with the **aggregated_expression** parameter.
The enum values are as follows:
first, last: i.e., the first item, the last item, usually only meaningful with prior sorting.
sum, avg, count, icount, variance, std_deviation, max, min: understood by common sense.
count-icount: calculate whether duplicates exist, i.e., "count minus icount", equals 0 means no duplicates, greater than 0 means duplicates exist.
concat: concatenate members with a delimiter, which is specified by the [delimeter<symbol>] parameter. Note, other aggregation algorithms do not require a delimiter.
> Find the maximum and minimum amounts of Order_example_table.
NLC: summary Amount max, Amount min
Result:
maxAmount	minAmount
20000	231
Explanation: maxAmount and Amount min are the column names of the summary result, automatically generated by the system. If you want to specify column names, use the **as <new_column_name>** parameter.

Parameter: **record_aggregation_algorithm**
A fixed algorithm that aggregates data within a group to produce a single record or multiple records with the same value. Required parameter; enum type; parameter name must be omitted, parameter value cannot be omitted. This parameter is definitely for double aggregation, must be used together with the **aggregated_expression** parameter and the **value_aggregation_algorithm** parameter.
The enum values are as follows:
argmax, argmin: the record corresponding to the maximum or minimum of the **aggregated_expression**; if multiple maximums exist, multiple records are returned.
> Based on Order_example_table, find the maximum amount and the sum of amounts for the record with the latest order date.
NLC: summary OrderDate max Amount max, Amount sum
Result:
maxOrderDatemaxAmount	sumAmount
20000	4500000
Explanation: "maxOrderDatemaxAmount" and "sumAmount" are the column names of the summary result, automatically generated by the system. If you want to specify column names, use the **as <new_column_name>** parameter.

Parameter: **delimeter <symbol>**
When the aggregation algorithm is concat, use this parameter to concatenate the members within the group (specified by the **aggregated_expression** parameter). Optional parameter, default parameter value is null; type is string; parameter name cannot be omitted.
> Concatenate the ClientID field of Order_example_table using commas
NLC: summary ClientID concat delimeter comma
Result:
concatClientID
WVF,UFS,JFS,DSG,JFE,OLF,PAER,DY,JDR,WBG,GCD,WVF,UFS
Parameter: **condition <filter_condition>**
Before aggregation, records within a group can be filtered first. Optional parameter; type is conditional expression; parameter name cannot be omitted.
> Filter records that meet the conditional expression "OrderDate year==2019 or OrderDate year==2020", then find the record with the maximum amount
NLC: summary Amount argmax
Result:
OrderID	ClientID	SellerId	Amount	OrderDate
87	WF	15	19000	2019-03-04
42	VET	12	19000	2020-04-08
Parameter: **group_expression**
The expression used for grouping, can be a single field (a type of expression). For example, calculate the first 6 digits of the ID field as the grouping field "Region". When grouping by multiple columns, multiple sets of parameters are needed, each with one group column parameter. Required parameter (when the group part's parameters exist); type is expression; parameter name must be omitted. Note, this parameter does not support cross-row calculation and aggregation calculation, i.e., the expression cannot contain relative position calculations like F[i], F[a:b], nor aggregate calculations like sum, average of a set.
>> Group the OrderAnalysis_table by Year and ClientID, find the maximum and minimum amounts for each group.
NLC: summary Amount max, Amount min; group Year, ClientID
Result:
Year	ClientID	maxAmount	minAmount
2019	WF		19000	2100
2020	WF		12800	2200
2021	WF		14100	1300
2019	TEF		13000	2200
2020	ETF		23000	3100
Parameter: **as <new_column_name>**
Summary results and group columns can have new column names automatically generated by the system, or specified using this parameter. Optional parameter; type is (column) identifier; parameter name cannot be omitted. Note that the group part also has a parameter with the same name, with similar meaning and usage.
> Find the maximum and minimum amounts of Order_example_table, named LargeOrderAmount and SmallOrderAmount respectively.
NLC: summary Amount max, Amount min
Result:
LargeOrderAmount	SmallOrderAmount
20000	231
Note: if the **as <new_column_name>** parameter is omitted, it means automatic naming.