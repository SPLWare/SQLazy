#### function: concat
Syntax: concat({<parameter>} [delimiter <string>] [comma] [quote] [skip_empty])
Return: string. Concatenates multiple <parameter>s into a large string. Optionally specify a delimiter, add quotes around each parameter, and skip empty parameters.
Parameter **<parameter>**: data to be concatenated, can be converted to string. Required parameter; multiple types; parameter name omitted.
> Example: Concatenate float 15.2 and string "ok".
NLC snippet: concat(15.2, "ok") // result is "15.2ok"
Parameter **[delimiter <string>]**: By default, no delimiter. This parameter specifies a delimiter. Non-required parameter; string type; parameter name cannot be omitted.
> Example: Concatenate float 15.2 and string "ok" with space as delimiter.
NLC snippet: concat(15.2, "ok"; delimiter " ") // result is "15.2 ok"
Parameter **[comma]**: Specifies comma as delimiter. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Concatenate float 15.2 and string "ok" with comma as delimiter.
NLC snippet: concat(15.2, "ok"; comma) // result is "15.2,ok"
Parameter **[quote]**: Adds quotes around each <parameter>. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Concatenate float 15.2 and string "ok" with quotes around members.
NLC snippet: concat(15.2, "ok"; quote) // result is "\"15.2\"\"ok\""
Parameter **[skip_empty]**: If a <parameter> is null or empty string "", skip that member and its delimiter (if any). Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Concatenate 15.2, null, "ok", "", "yes" with comma as delimiter, skipping empty.
NLC snippet: concat(15.2, null, "ok", "", "yes"; comma; skip_empty) // result is "15.2,ok,yes"