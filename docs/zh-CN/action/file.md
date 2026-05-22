### Action: file
Syntax: <file>  [sheet <sheet_name>]  [delimeter <symbol>]  [csv]  [header]
Parameter: **file**
The file name and path to read. Required parameter; string type; parameter name must be omitted. Path is separated by "\\" or "/"; supports absolute paths.
> Example: read the tab-separated text file C:\1\Orders.txt, first row is the header.
NLC: file "C:\\1\\Orders.txt"; header
Explanation: "C:\\1\\Orders.txt" is the parameter value of the file parameter, the parameter name has been omitted.
Results (examples only list partial results, same below):
OrderID	ClientID	SellerId	Amount	OrderDate
1	WVF	5	440.0	2022-01-04
2	UFS	13	1863.4	2022-01-08
4	JFS	27	670.8	2022-01-12
5	DSG	15	3730.0	2022-01-15
6	JFE	10	1444.8	2022-01-19

The file parameter supports relative paths relative to the main directory in NLC configuration items.
> NLC:
file "1\\Orders.txt"; header
file "1/Orders.csv"; header

Parameter: **sheet**
If the file is Excel, you can specify the sheet name to load. Optional parameter, default (when this parameter is absent) loads the first sheet; string type; parameter name cannot be omitted.
> Example: read Sheet3 of the Excel, first row is column names
NLC: file "C:\\Orders.xls"; sheet Sheet3; header
Parameter: **delimeter**
If the file is a text file, you can specify the delimiter between columns. Optional parameter, default (when this parameter is absent) is tab; string type; parameter name cannot be omitted.
> Example: read a text file with semicolon as delimiter, first row as header
NLC: file "C:\\Orders.txt"; delimeter semicolon; header
Parameter: **csv** 
Whether to use comma as the delimiter. Optional parameter, equivalent to "delimeter comma", mutually exclusive with the parameter "delimeter"; boolean type (parameter name cannot be omitted, no parameter value).
> Example: read a comma-separated text file, header in the first row.
NLC: file "C:\\Orders.csv"; csv; header
Parameter: **header**	
Whether to recognize the first row as the header (i.e., column names/field names). Optional parameter, default does not recognize the first row as header, in which case fields are automatically named _1, _2, _3…; boolean type (parameter name cannot be omitted, no parameter value).
> Example: read a text file without a header row, comma-separated.
NLC: file "C:\\Orders.txt"; csv;
Results:
_1	_2	_3	_4	_5
1	WVF	5	440.0	2022-01-04
2	UFS	13	1863.4	2022-01-08
4	JFS	27	670.8	2022-01-12
5	DSG	15	3730.0	2022-01-15
6	JFE	10	1444.8	2022-01-19