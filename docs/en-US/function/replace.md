#### function: replace
Syntax: (<object_parameter> replace [from <marker_substring>] [to <new_substring>] [first_only] [nocase] [skip_quoted] [whole])
Return: string. Replaces all occurrences of [from <marker_substring>] with [to <new_substring>] in <object_parameter> and returns the result. Optionally replace only the first occurrence, case-insensitive, skip quoted content, or replace only whole words (delimited by symbols).
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **[from <marker_substring>]**: substring to be replaced. Required parameter; string type; parameter name omitted.
Parameter **[to <new_substring>]**: new substring to replace with. Required parameter; string type; parameter name omitted.
> Example: Replace all occurrences of "a" with "China" in "abcabcdef".
NLC snippet: ("abcabcdef" replace from "a", to "China") // result is ChinabcChinabcdef
Parameter **[first_only]**: By default, replace all occurrences. With this parameter, replace only the first. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Replace the first occurrence of "a" with "China" in "abcabcdef".
NLC snippet: ("abcabcdef" replace from "a", to "China"; first_only) // result is Chinabcabcdef
Parameter **[nocase]**: Default is case-sensitive. With this parameter, ignore case. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
> Example: Replace occurrences of "A" with "China" in "abcabcdef", case-insensitive.
NLC snippet: ("abcabcdef" replace from "A", to "China"; nocase) // result is ChinabcChinabcdef
Parameter **[skip_quoted]**: By default, content inside quotes is also replaced. With this parameter, content inside quotes (single or double quotes) is skipped and not replaced. Optional parameter; boolean type; parameter name must not be omitted, parameter value must be omitted.
> Example: Replace "a" with "China" in "abc'abc'def", skipping quoted content.
NLC snippet: ("abc'abc'def" replace from "a", to "China"; skip_quoted) // result is Chinabc'abc'def
Parameter **[whole]**: By default, the source string is not tokenized; any occurrence of [from <marker_substring>] is replaced. For example, "aa" appears 3 times in "aa,aaa". With this parameter, the source string is split into tokens by delimiters, and replacement occurs only when [from <marker_substring>] matches a whole token. Non-required parameter; boolean type; parameter name cannot be omitted, parameter value must be omitted.
Example: Replace "aa" with "AA" in "aaa,aab,aa", only whole words.
NLC snippet: ("aaa,aab,aa" replace from "aa", to "AA"; whole) // result is aaa,aab,AA