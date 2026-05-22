### Action: distinct
Description: Based on one or more specified columns, keep unique records. Optionally delete records with duplicates, delete non-duplicate records.
Syntax: <column> <option> [partition <partition_field>]
Parameter: **column**
One or more columns used to determine duplicates. Required parameter, when absent means the parameter value is the focus column in the context; type is identifier or set of identifiers; parameter name must be omitted.
Parameter: **option**
Three ways of deduplication. Optional parameter; enum type; parameter name must be omitted, parameter value cannot be omitted. Enum values are as follows:
keep_unique: default value, when N records are duplicates, remove the 2nd to Nth records, keep only the 1st.
kill_dups: delete records with duplicates, equivalent to keeping only records that are singletons.
dups_only: delete non-duplicate records, i.e., delete all singleton records.
> Example: Supermarket order example table as follows
OrderID	Product	Salesperson	OrderAmount
1	Watermelon	Zhang San	100
2	Watermelon	Zhang San	200
3	Watermelon	Zhang San	300
4	Apple	Zhang San	400
5	Apple	Zhang San	500
6	Watermelon	Li Si	600
Goal: remove duplicate records based on Product and Salesperson fields
NLC: distinct	Product,Salesperson
Result:
OrderID	Product	Salesperson	OrderAmount
1	Watermelon	Zhang San	100
4	Apple	Zhang San	400
6	Watermelon	Li Si	600

> Based on the above supermarket order example table, delete records with duplicates based on Product and Salesperson fields.
NLC: distinct	Product,Salesperson; kill_dups
Result:
OrderID	Product	Salesperson	OrderAmount
6	Watermelon	Li Si	600

> Based on the above supermarket order example table, delete non-duplicate records based on Product and Salesperson fields.
NLC: distinct	Product,Salesperson; dups_only

Result:
OrderID	Product	Salesperson	OrderAmount
1	Watermelon	Zhang San	100
2	Watermelon	Zhang San	200
3	Watermelon	Zhang San	300
4	Apple	Zhang San	400
5	Apple	Zhang San	500
Parameter: **partition**
Deduplicate by partition, partitions do not affect each other, conceptually similar to SQL's PARTITION BY. Optional parameter; identifier type; parameter name cannot be omitted.