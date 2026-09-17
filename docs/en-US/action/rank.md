### Action: rank

Note that this action does not change the record order, nor does it return sorted records.

Syntax: {<rank_by> [direction]} [option] [as <new_column_name_for_rank>] [filter] <filter_count> [delete] [partition <partition_field>]

Parameter: **<rank_by>** **[direction]**

The parameter <rank_by> indicates what to internally sort by, based on which rankings are then assigned. This parameter is the expression on which ranking is based; the simplest expression can be a single field name. It does not support cross-row and aggregation calculations, i.e., the expression cannot contain relative position calculations like F[i], F[a:b], nor aggregate calculations like sum, average on record sets. Type is expression; parameter name must be omitted.

The parameter [direction] is the direction of the ranking basis. Optional parameter; type is enum, enum values are (asc|small_to_large)|(desc|large_to_small); parameter name must be omitted, parameter value cannot be omitted, the default parameter value is asc|small_to_large.

**Important Rule**: The <rank_by> and [direction] parameters can both be absent, meaning ranking by the original record order. If these parameters appear, they can be in pairs, or only <rank_by> with [direction] defaulted. Multiple pairs can appear, meaning calculating rank based on the sequential sorting result.

> Rank the Order_example_table (focus table) by the expression (len(customer)) descending, then OrderAmount ascending, assign the rank to the new field rankall.

SQLazy: rank (len(customer)) desc, OrderAmount; as rankall

Parameter: **as <new_column_name_for_rank>**

When the rank needs to be written into a new column, this parameter should be used. Optional parameter; type is field identifier; parameter name cannot be omitted. Note, this parameter and the subsequent parameter **filter** cannot be used together; when not using this parameter, the subsequent parameter **filter** must be used to return partial records, without changing the table structure.

> Rank the Order_example_table (focus table) by original order, assign to a new field "RowField" (equivalent to explicitly naming the row number column #)

SQLazy: rank as RowField

Parameter: **option**

When encountering duplicate values, there are several rules to differentiate whether the ranks are duplicated, and whether they occupy the ranks of new values. Only 2 enum values can be written: position, dense; in addition there is 1 default rule that cannot be written, i.e. what this prompt calls "American" (it is not a SQLazy keyword and must never appear in the output). Optional parameter, default parameter value is "American" (generally a default value may be written or not, but the default "American" is special, it has no corresponding enum value, so this parameter must be omitted); type is enum; parameter name must be omitted, parameter value cannot be omitted. When the user says "rank by the American rule" or "use the default rule" without specifying position or dense, always omit this parameter. The enum values are as follows:

-	position: no duplicate ranks, the natural ranking of [10,10,20,25,30,30] is [1,2,3,4,5,6]. The top 4 are [10,10,20,25]
-	the default "American" rule (omit this parameter): duplicate values share the same rank, but occupy the ranks of new values; the "American" ranking of [10,10,20,25,30,30] is [1,1,3,4,5,5]. The top 4 are [10,10,20,25]
-	dense: duplicate values share the same rank, and do not occupy the ranks of new values; the dense ranking of [10,10,20,25,30,30] is [1,1,2,3,4,4]. The top 4 are [10,10,20,25,30,30]

> Rank by OrderAmount, Chinese rule, name it AmountRank.

SQLazy: rank OrderAmount; as AmountRank; dense

> Rank by OrderAmount, name it AmountRank.	//Omit the option, i.e. the default "American" rule

SQLazy: rank OrderAmount; as AmountRank

Parameter: **filter** **filter_count**

These two parameters are usually used together, meaning filtering the ranking result to return only partial records, i.e., top N, bottom N, the maximum one, the minimum one.

Where, parameter **filter** is the direction of filtering; optional parameter; type is enum; parameter name must be omitted, parameter value cannot be omitted. When using the **as** parameter, this parameter cannot be used, meaning filtering by rank and writing rank to a new column are mutually exclusive. The 4 enum values are as follows:

-	top, bottom: top N or bottom N, must be used together with the **filter_count** parameter
-	max, min: the record corresponding to the maximum value, the record corresponding to the minimum value. Note that for max and min, it is equivalent to fixing the value of the **direction** parameter, so **direction** can be omitted; the record corresponding to the max (and similarly for min) may be multiple records if the maximum value is duplicated. In this case, the return is based on the **filter_count** parameter. If **filter_count** is 1, only the first record is returned; if there is no **filter_count** parameter, all records corresponding to the maximum value are returned. Same for min.

Where, parameter **filter_count** is the number of ranks (for top/bottom) or the number of records (for max/min). Required parameter, default value is 1; type is integer or integer identifier; parameter name must be omitted, parameter value cannot be omitted.

> Focus table ranked by OrderAmount, retrieve all records corresponding to the maximum value

SQLazy: rank OrderAmount; max

> Focus table ranked by OrderAmount, retrieve the record corresponding to the 1st place

SQLazy: rank OrderAmount; top; 1	//Default option "American"

> Focus table ranked by OrderAmount in descending order, using Chinese rule, retrieve the records corresponding to the top 10.

SQLazy: rank OrderAmount; desc; dense; top; 10

Parameter: **[delete]**

When using the **filter** parameter, the default is to select records that meet the condition based on ranking. If this parameter is used, it means deleting the records that meet the condition based on ranking. Optional parameter; type is boolean; parameter name cannot be omitted, parameter value must be omitted.

> Focus table ranked by OrderAmount in descending order, using Chinese rule, delete the records corresponding to the top 10.

SQLazy: rank OrderAmount; desc; dense; top; 10; delete

Parameter: **partition**

Rank by partition, partitions do not affect each other, conceptually similar to SQL's PARTITION BY. Optional parameter; type is identifier; parameter name cannot be omitted, parameter value cannot be omitted.

> Example: Order_example_table already sorted by Amount ascending, now retrieve the bottom 3 of each customer's order amounts (equivalent to retrieving the top 3 largest order amounts for each customer)

SQLazy: rank OrderAmount; bottom; 3; partition Customer
