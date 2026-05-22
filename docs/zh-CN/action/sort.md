### Action: sort
**Focus Table**
Syntax: {<expression> [direction]}  [null] [orig_order] [language]
Parameter: **expression**
The expression to sort by, the simplest expression is a single field. Required parameter; type is expression; parameter name must be omitted, when the parameter value is omitted, it means the parameter value is the focus column in the context. Generally used in pairs with the "direction" parameter, meaning sorting a field in a certain direction, supporting multiple such pairs, which means sorting by multiple fields in sequence. This parameter does not support cross-row calculation and aggregation calculation, i.e., the expression cannot contain relative position calculations like F[i], F[a:b], nor aggregate calculations like sum, average of a set.
Parameter: **direction**
The direction of sorting. Optional parameter; enum type, enum values are (asc|small_to_large)|(desc|large_to_small); parameter name must be omitted, the default parameter value is asc|small_to_large. Note, must be used together with the "expression" parameter.
> Example: sort Order_example_table by ClientID ascending, Amount descending.
sort ClientID, Amount desc
Result:
OrderID	ClientID	SellerId	Amount	OrderDate
136	ARO	25	899.0	2024-09-23
16	BDR	27	2464.8	2022-04-30
81	BDR	29	1168.0	2023-08-25
108	BDR	12	480.0	2024-04-03
139	BDR	30	166.0	2024-10-11
Parameter: **null**
When sorting, you can choose to place null values at the front or back. Optional parameter, default determined by system configuration; enum type; parameter name must be omitted, parameter value cannot be omitted. Two enum values:
- first	 means placing null values first
- last means placing null values last
> Example: sort Order_example_table by ClientID, place null ClientID last
NLC: sort ClientID; last
Parameter: **orig_order** 
Sort the records by the order in which the original values of a certain column appeared. Optional parameter, without this parameter, it sorts by the magnitude of the column values (i.e., normal sorting); boolean type, parameter name cannot be omitted, parameter value must be omitted. When the orig_order parameter is present, the "direction" parameter is meaningless.
The difference with and without this parameter is illustrated below.
> The Region_table before sorting is as follows
c1	f1	f2	f3
1	China		
2	China	Henan	
3	China	Hebei	
4	China	Henan	Luoyang
5	China	Hebei	Langfang
6	China	Hebei	Cangzhou
7	China	Henan	Luoyang
8	China	Hebei	
9	China	Hebei	Cangzhou

If sorted by f2 ascending, Hebei will come before Henan
NLC: sort f2
Result:
sort f2  //Description, sort f2 ascending, Hebei comes before Henan
c1	f1	f2	f3
1	China		
3	China	Hebei	
5	China	Hebei	Langfang
6	China	Hebei	Cangzhou
8	China	Hebei	
9	China	Hebei	Cangzhou
2	China	Henan	
4	China	Henan	Luoyang
7	China	Henan	Luoyang

If sorted by f2's original order, Henan will come before Hebei:
NLC: sort f2 orig_order
Result:
c1	f1	f2	f3
1	China		
2	China	Henan	
4	China	Henan	Luoyang
7	China	Henan	Luoyang
3	China	Hebei	
5	China	Hebei	Langfang
6	China	Hebei	Cangzhou
8	China	Hebei	
9	China	Hebei	Cangzhou

If sorted by the original order of f2, f3,
NLC: sort f2, f3 orig_order
Result:
c1	f1	f2	f3
1	China		
2	China	Henan	
4	China	Henan	Luoyang
7	China	Henan	Luoyang
3	China	Hebei	
8	China	Hebei	
5	China	Hebei	Langfang
6	China	Hebei	Cangzhou
9	China	Hebei	Cangzhou

Parameter: **language** 
Specify the language of the string in sorting. Optional parameter, default uses the local language when absent; string type; parameter name cannot be omitted.
> Example: the field being sorted is in English.
NLC: sort ClientID; language en  //Common languages also include zh (Chinese), ja_JP (Japanese), etc.