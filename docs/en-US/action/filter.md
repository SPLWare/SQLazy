### Action: filter
Syntax: <condition> [delete] [partition <partition_field>]
Parameter: **condition**
Conditional expression. If the current record makes the conditional expression true, return that record. Required parameter; parameter name must be omitted. This parameter supports cross-row calculation and aggregation calculation, i.e., the expression can contain relative position calculations like F[i], F[a:b], and aggregate calculations like sum, average of a set.
> Example: filter records, condition: State equals "New York" or State equals Texas
NLC: filter (State equals "New York" or State equals Texas)
Parameter: **delete**
Means deleting records that meet the condition from the table and returning the remaining records. Optional parameter, default without this parameter returns records meeting the condition; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: delete records that do not meet the condition, conditional expression: State equals "New York" or State equals Texas
NLC: filter (State equals "New York" or State equals Texas); delete
Parameter: **partition**
Filter by partition, partitions do not affect each other (e.g., cross-row conditions cannot cross into other partitions), conceptually similar to SQL's PARTITION BY. Optional parameter; identifier type; parameter name cannot be omitted.
> Example: filter records within each department, condition: State equals "New York" or State equals Texas; 
NLC: filter (State equals "New York" or State equals Texas); partition Department