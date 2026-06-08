### Action: sort
**Focus Table**
Syntax: {<expression> [direction]}  [null] [language]
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
Parameter: **language** 
Specify the language of the string in sorting. Optional parameter, default uses the local language when absent; string type; parameter name cannot be omitted.
> Example: the field being sorted is in English.
NLC: sort ClientID; language en  //Common languages also include zh (Chinese), ja_JP (Japanese), etc.