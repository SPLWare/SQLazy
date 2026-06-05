### Action: list
Syntax: [from <ordered_set_start>] [to <ordered_set_end>] [step <step_size>] [unit] [as <single_column_table_field_name>] [no_end]
Parameter: **from** Parameter: **to**
These two parameters are generally used together, representing the lower and upper bounds of the ordered set. Generate an ordered set starting from **from** until **to**. **from** is not a required parameter, default is 1; **to** is a required parameter; date-time, integer (or expression that evaluates to date-time/integer), the data types of **from** and **to** must be the same; parameter name cannot be omitted.
> Example: generate ordered set [1,2,3,4,5,6,7]
NLC: list to 7
> Example: generate ordered set [3,4,5,6,7]
NLC: list from 3, to 7
Parameter: **step**
The interval when generating the ordered set, default is 1, i.e., generate sequentially. Optional parameter, default step is 1; type same as from/to; parameter name cannot be omitted.
> Example: generate ordered set [3,5,7]
NCL: list from 3, to 7; step 2
Parameter: **unit**
When parameters **from** and **to** are time (date, datetime), this parameter should be used to specify the unit of **step**. Optional parameter; enum type, enum values are year, quarter, month, day, hour, minute, second; parameter name must be omitted, parameter value cannot be omitted.
> Example: generate ordered set [2021-01-03, 2021-01-05, 2021-03-07]
NLC: list from 2021-01-03, to 2021-01-07; step 2; day
Different time units result in different intervals between members of the ordered set.
> Example: generate set [2021-01-01 2021-03-01, 2021-05-01].
NCL: list from 2021-01-01, to 2021-05-01; step 2; month

Parameter: **as**
Specify the field name of the single-column table; identifier. Optional parameter, without this parameter it means generating an ordered set, with this parameter it means generating a table; string type; parameter name cannot be omitted.
> Example: generate a table with 3 records, having a single field name F1, with field values 3,5,7 respectively, or the column values of F1 are [3,5,7].
NCL: list to 7; step 2; as F1
Rule: Many actions have an "as" parameter. Unless otherwise specified, its meaning is uniformly "the new field name (identifier)", not explained repeatedly.
Parameter: **no_end**
Discard the last member of the ordered set. Optional parameter, default does not discard; boolean type (parameter name cannot be omitted, no parameter value)
