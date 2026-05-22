#### function: findsubs
Syntax: (<object_parameter> findsubs(<substring> [nth <nth_substring>] [nocase] [reverse] [whole] [skip_quoted]))
Return: boolean. Returns true if <object_parameter> contains <substring>, otherwise false. Optionally find the Nth occurrence, case sensitivity, search direction, whole word matching (delimited by symbols), and skip quoted content.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<substring>**: string to search for. Required parameter; string type; parameter name omitted.
> Example: Check if "中国人民电视台" contains the substring "人民".
NLC snippet: ("中国人民电视台" findsubs("人民")) // returns true.
Parameter **[nth <nth_substring>]**: The substring may appear multiple times. This parameter restricts to the Nth occurrence. Non-required parameter; integer type; parameter name cannot be omitted.
> Example: Check if "两只老虎两只老虎跑得快" contains the second occurrence of "两只老虎", and the third occurrence.
NLC snippet: ("两只老虎两只老虎跑得快" findsubs("两只老虎"; nth 2)) // returns true
NLC snippet: ("两只老虎两只老虎跑得快" findsubs("两只老虎"; nth 3)) // returns false
Parameter **nocase**: case-insensitive matching. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Check if "abcdef" contains substring "Def", case-insensitive.
NLC snippet: ("abcdef" findsubs("Def"; nocase)) // returns true
Parameter **reverse**: Search from the end instead of the beginning (default is forward). Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
Parameter **whole**: By default, the source string is not tokenized; any occurrence counts. For example, "aa" appears 3 times in "aa,aaa". When this parameter is present, the source string is split into tokens by delimiters, and only a complete match of the substring as a whole token returns true. For example, in "aa,aaa", there is only one whole-word "aa". Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Check if the second occurrence of "aa" appears as a whole word in "aa,aaa".
NLC snippet: ("aa,aaa" findsubs("aa"; nth 2; whole)) // returns false
Parameter **[skip_quoted]**: Skip quoted content; do not match inside quotes. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Check if "中国\'人民广播\'电视台" contains substring "人民", skipping quoted parts.
("中国\'人民广播\'电视台" findsubs("人民"; skip_quoted)) // returns false