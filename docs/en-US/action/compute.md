### Action: compute
Syntax: {[summary] <formula> [with] [cross] [as <new_column_name>]} [partition]
Key Rules
-	The parameters [summary] and [cross] are mutually exclusive, only one can be chosen, cannot appear together.
-	When <formula> is of the relative interval F[a:b] form (a != b), the value is a set, and this parameter must be used (the parameter [cross] cannot be used); when <formula> is not of the relative interval form, this parameter is not mandatory (i.e., choose one between this parameter and [cross]).
-	{} indicates that it can be repeated multiple times. In the syntax of this action, it specifically means multiple computed columns can be created, each with its own set of parameters [summary] <formula> [with] [cross] [as <new_column_name>]. Different computed columns and <partition> are separated by semicolons.
-	The parameter structure corresponds to the table-type parameter [compute]: when field names are omitted, the field order follows the same rule as the parameter order, matching by type and agreed order (corresponding to spec L96 new sentence).
Parameter: **[summary]** 
**[summary]** is to continue with an aggregation calculation based on the parameter <formula>. Within the same partition, non-relative-interval (non-set) aggregation produces the same result for each row; relative-interval (set) aggregation usually produces different results per row. Without the partition parameter, it can be considered as having only one partition. Note that any <formula> can use this parameter; when <formula> is of the relative interval F[a:b] form (a != b), the value is a set, and this parameter must be used (the parameter [cross] cannot be used), e.g., OrderAmount[-2:1], which represents the set of OrderAmount from the 2nd record before the current position to the 1st record after, a total of 4 records; when the <formula> is not of the relative interval form, this parameter is not mandatory (i.e., choose one between this parameter and [cross]), e.g., OrderAmount, OrderAmount*0.1, UnitPrice*Quantity. Optional parameter; enum type; parameter name must be omitted, parameter value cannot be omitted. The explanations of the enum values are as follows:
first, last: the first and last item respectively, often require prior sorting.
sum, avg, count, icount, max, min: understood by common sense. When the parameter value (enum value) of **[summary]** is count, the <formula> can be omitted, to comply with SQL conventions.
concat: use the [with] parameter as a delimiter to concatenate the <formula> into a large string. This is a special summary calculation.
count-icount: i.e., "count minus icount", equals 0 means no duplicates, greater than 0 means duplicates exist.
> Calculate the 5-day moving average for a stock price table, fill into new column MA5.
NLC: compute avg, ClosePrice[-2:2], as MA5
> Example: for each salesperson in Order_example_table, use the Amount field to calculate the total order amount for each salesperson, and fill it into the new column "TotalAmountPerSeller" for each record.
NLC: compute sum, Amount, as TotalAmountPerSeller; partition SellerID
Partial results:
OrderID	Client	SellerId	Amount	OrderDate	TotalAmountPerSeller
26	TAS	1	2142.4	2009-08-05	2755.6000000000004
33	DSGC	1	613.2	2009-08-14	2755.6000000000004
32	JFS	3	468.0	2009-08-13	3484.0
39	NR	3	3016.0	2010-08-21	3484.0
> Example: Based on the previous example, continue calculating, write the result of the expression "TotalAmountPerSeller*0.1" into a new column "Reward".
NLC: compute TotalAmountPerSeller*0.1, as Reward		//The result is the same whether partitioned or not.
Note that the above two NLC statements can be combined into the following one (not all cases can be substituted), changing the calculation order but the result remains the same.
NLC: compute sum, Amount*0.1, as Reward; partition ClientID

Parameter: **<formula>**
Usually an expression based on one or more fields, also allowing special expressions like constants and null values. Required parameter; expression type; parameter name must be omitted.
> Example: calculate the target amount for Order_example_table using the expression "if ((OrderDate_year=2022 and OrderDate_month=3) then Amount*1.1)".
NLC: compute (if ((OrderDate_year=2022 and OrderDate_month=3) then Amount*1.1)) , as TargetAmount
Parameter: **[with]**
When the [summary] value is "concat", this parameter specifies the delimiter (string) used for concatenation. Optional parameter, default is empty string ""; string type; parameter name cannot be omitted, parameter value cannot be omitted. Note, this parameter is only needed when [summary] is concat; other enum values do not need this parameter.
 > Example: Partition Order_example_table by SellerId, concatenate the Client field with a comma into a large string, name the new column "ClientList"
NLC: compute concat, Client, with ",", as ClientList; partition SellerId
Partial results:
OrderID	Client	SellerId	Amount	OrderDate	ClientList
26	TAS	1	2142.4	2009-08-05	TAS,DSGC,GC,HU
33	DSGC	1	613.2	2009-08-14	TAS,DSGC,GC,HU
84	GC	1	88.5	2009-10-16	TAS,DSGC,GC,HU
133	HU	1	1419.8	2010-12-12	TAS,DSGC,GC,HU
32	JFS	3	468.0	2009-08-13	JFS,NR,KT,JFE,RA
39	NR	3	3016.0	2010-08-21	JFS,NR,KT,JFE,RA
43	KT	3	2169.0	2009-08-27	JFS,NR,KT,JFE,RA
71	JFE	3	240.4	2010-10-01	JFS,NR,KT,JFE,RA
99	RA	3	1731.2	2009-11-05	JFS,NR,KT,JFE,RA
Parameter: **[cross]**  Performs cross-row calculation based on the parameter <formula>. Unlike aggregation, the calculation result for each row within a partition is different. Optional parameter; enum type; parameter name must be omitted, parameter value cannot be omitted. The enum values are as follows:
proportion: understood by common sense.
inc, inc_ratio: inc_ratio is the growth ratio of the current item compared to the previous item, value/value[-1]-1.
cum, cum_proportion: cum_proportion is the proportion of the current cumulative value to the total sum.
> Example: calculate the proportion of each customer's order amount to the total amount of that customer in Order_example_table.
NLC: compute Amount, proportion, as AmountProportion; partition ClientID
Partial results:
OrderID	ClientID	SellerId	Amount	OrderDate	AmountProportion
136	ARO	25	899.0	2024-09-23	1.0
16	BDR	27	2464.8	2022-04-30	0.5760493596335421
81	BDR	29	1168.0	2023-08-25	0.2729737309526035
108	BDR	12	480.0	2024-04-03	0.11218098532298774
139	BDR	30	166.0	2024-10-11	0.038795924090866594
93	BON	6	2564.4	2023-11-04	1.0
14	BSF	26	448.0	2022-04-12	0.031124079477560095
79	BSF	9	982.0	2023-08-10	0.06822287064054468
106	BSF	27	10741.6	2024-03-10	0.7462553841878561
137	BSF	18	2222.4	2024-09-26	0.15439766569403918
> Example: create multiple computed columns, newField1 is the proportion of each customer's order amount to the total amount of that customer, newField2 is the average of each customer's order amount.
NLC: compute Amount, proportion, as newField1; avg, Amount, as newField2; partition ClientID
Parameter: **[as <new_column_name>]**
The new column name created for the computed column. Required parameter; type is column identifier; parameter name cannot be omitted. Note: the new column name should not duplicate the original name, otherwise there will be two identical names, causing confusion in subsequent references.
Parameter: **[partition]**
Calculate by partition, partitions do not affect each other, similar to SQL's PARTITION BY. Optional parameter; identifier type; parameter name cannot be omitted.
> Example: For Order_example_table already sorted by OrderDate, for each customer's records use the expression " Amount[-1]*1.1" to perform cross-row calculation, and fill the result into the new column TargetAmount, ensuring customers do not affect each other.
NLC: compute Amount[-1]*1.1, as TargetAmount; partition ClientID