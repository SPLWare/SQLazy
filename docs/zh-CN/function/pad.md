#### function: pad
Syntax: (<object_parameter> pad([with <substring>] [length <total_length>] [right]))
Return: string. Repeatedly prepends <substring> to <object_parameter> until the total length reaches <total_length>, and returns the resulting string. Optionally append the substring to the right.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **[with <substring>]**: substring to be padded. Required parameter; string type; parameter name cannot be omitted.
Parameter **[length <total_length>]**: The target total length after padding. Required parameter; integer type; parameter name cannot be omitted.
> Example: Repeatedly prepend "Miss" to "Soth" until total length reaches 10.
NLC snippet: ("Soth" pad(with "Miss", length 10)) // result is "MissMiSoth"
Parameter **[right]**: By default, pad on the left. With this parameter, pad on the right. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Repeatedly append "Miss" to "Soth" until total length reaches 10.
NLC snippet: ("Soth" pad(with "Miss", length 10; right)) // result is "SothMissMi"