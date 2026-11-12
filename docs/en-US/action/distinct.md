### Action: distinct
Description: Determine duplicates based on one or more specified columns and keep unique records. By default only those columns and the partition columns are returned; to keep the other columns of the whole row, write whole. Optionally delete records with duplicates, delete non-duplicate records.
Syntax: <column> <option> [whole] [partition <partition_field>]
Parameter: **column**
One or more columns used to determine duplicates. Required parameter; type is identifier or set of identifiers; parameter name must be omitted.
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
SQLazy: distinct	Product,Salesperson
Result:
Product	Salesperson
Watermelon	Zhang San
Apple	Zhang San
Watermelon	Li Si

> Based on the above supermarket order example table, remove duplicate records based on Product and Salesperson fields, keeping the other columns of the whole row.
SQLazy: distinct	Product,Salesperson; whole
Result:
OrderID	Product	Salesperson	OrderAmount
1	Watermelon	Zhang San	100
4	Apple	Zhang San	400
6	Watermelon	Li Si	600

> Based on the above supermarket order example table, delete records with duplicates based on Product and Salesperson fields.
SQLazy: distinct	Product,Salesperson; kill_dups; whole
Result:
OrderID	Product	Salesperson	OrderAmount
6	Watermelon	Li Si	600

> Based on the above supermarket order example table, delete non-duplicate records based on Product and Salesperson fields.
SQLazy: distinct	Product,Salesperson; dups_only; whole

Result:
OrderID	Product	Salesperson	OrderAmount
1	Watermelon	Zhang San	100
2	Watermelon	Zhang San	200
3	Watermelon	Zhang San	300
4	Apple	Zhang San	400
5	Apple	Zhang San	500
Parameter: **whole**
Whether to keep the whole row. Optional parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted. When omitted, only the columns specified by "column" and the partition columns are returned, in the original table's column order; when written, all columns of the original table are returned, also in the original table's column order.
Parameter: **partition**
Deduplicate by partition, partitions do not affect each other, conceptually similar to SQL's PARTITION BY. Optional parameter; identifier type; parameter name cannot be omitted. Partition columns always appear in the result; if a partition column is also a duplicate-judging column, that column appears twice in the result.
