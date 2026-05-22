### Action: SQL
Syntax: <SQL>  [db <database_connection_name>] [DQL]
Parameter: **SQL**
The database structured query language. Required parameter; string type; parameter name must be omitted.
Parameter: **db**
Corresponds to the database connection name in NLC configuration items, which includes necessary connection information such as URL, driver, username, password. Optional parameter, default is the first database connection name; type is string, including expressions that evaluate to a string; parameter name must be omitted.
> Example: access the database using connection name orcl, execute SQL, query the Order_example_table and generate the corresponding structured data.
NLC: SQL "select * from Orders where Amount>1000 and Amount<=10000"; orcl
Results:
OrderID	ClientID	SellerId	Amount	OrderDate
1	WVF	5	1440.0	2022-01-04
2	UFS	13	1863.4	2022-01-08
4	JFS	27	1670.8	2022-01-12
5	DSG	15	3730.0	2022-01-15
6	JFE	10	1444.8	2022-01-19
Parameter: **DQL**
Indicates that the parameter **SQL** uses DQL syntax. DQL is a SQL-like syntax; you don't need to care about its specific details. Optional parameter; boolean type; parameter name cannot be omitted, no parameter value.
> Example: execute a DQL
NLC: SQL "SELECT SUM(buss_zwsc), AVG(fee_rate) ON dim_time, dim_company FROM fact_package"; DQL  
Explanation: The data source name is omitted in the above NLC code.