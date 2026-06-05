### Action: table
Syntax: {[<expression>] [as <field_name>]}
Parameters: 
The two parameters usually appear together, representing a sequence of all values of a field and the field name, repeated several times for several fields; both are optional parameters; parameter "{[expression]" type is an expression that evaluates to a set, including set constants, parameter "as" type is identifier, multiple pairs form a "key-value" table; parameter "{[expression]" must omit the parameter name, parameter "as" cannot omit the parameter name. Note, each column will be padded according to the set with the most members, so the number of records equals the length of the longest sequence.
> Example: generate an Order_example_table with 4 fields using ordered sets [1001,1002], ["SVF", "VNV", "TNC"], [2021-01-01, 2021-01-03], [3131.34, 2121.8], field names are OrderID, ClientID, OrderDate, Amount.
NLC:
table [1001,1002] as OrderID, ["SVF", "VNV", "TNC"] as ClientID, [2021-01-01, 2021-01-03] as OrderDate, [3131.34, 2121.8] as Amount
Note that after padding to the longest ClientID, it has 3 records, result:
OrderID	ClientID	OrderDate	Amount
1001	"SVF"	2021-01-01	3131.34
1002	"VNV"	2021-01-03	2121.8
"TNC"
