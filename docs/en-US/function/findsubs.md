#### function: findsubs
Syntax: (<object_parameter> findsubs <substring> [nth <start_position>] [nocase] [whole])
Return: boolean. Returns true if <object_parameter> contains <substring>, otherwise false. Optionally specify a starting character position, case insensitivity, and whole word matching (delimited by symbols).
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<substring>**: string to search for. Required parameter; string type; parameter name omitted.
> Example: Check if string "中国人民电视台" contains substring "人民".
SQLazy snippet: ("中国人民电视台" findsubs "人民")					// returns true.
Parameter **[nth <start_position>]**: Start searching from the Nth character of the object parameter. Non-required parameter; integer type; parameter name cannot be omitted.
> Example: Starting from the 5th character of "两只老虎两只老虎跑得快", check if substring "两只老虎" appears.
SQLazy snippet: ("两只老虎两只老虎跑得快" findsubs "两只老虎"; nth 5)	// returns true
SQLazy snippet: ("两只老虎两只老虎跑得快" findsubs "两只老虎"; nth 6)	// returns false
Parameter **nocase**: Case-insensitive matching. Non-required parameter; boolean type; parameter name cannot be omitted; parameter value must be omitted.
> Example: Check if "abcdef" contains substring "Def", case-insensitive.
SQLazy snippet: ("abcdef" findsubs "Def"; nocase)	// returns true
Parameter **whole**: By default, the source string is not tokenized; any occurrence counts. For example, "aa" appears 3 times in "aa,aaa". When this parameter is present, the source string is split into tokens by delimiters, and only a complete match of the substring as a whole token returns true. For example, in "aa,aaa", there is only one whole-word "aa". Non-required parameter; boolean type; parameter name cannot be omitted; parameter value must be omitted.
> Example: Check if the string "aa,aaa" contains the second occurrence of "aa", matching only whole words.
SQLazy snippet: ("aa,aaa" findsubs "aa"; nth 2; whole)		// returns false
