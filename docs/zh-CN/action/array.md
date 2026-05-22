### Action: array
Syntax: [<formula>]
Parameter: **[<formula>]**
Usually an expression based on one or more fields of the original table, including field identifier, constant, null. Optional parameter, when omitted means the parameter value is the focus column in the context environment; expression type; parameter name must be omitted.
> Example: calculate each record using the expression Amount*1.1.
NLC: array Amount*1.1
Result: a set like [484, 2049.74, 737.88, 4103, 1589.28]