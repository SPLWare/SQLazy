#### function: date
Syntax: date(<year_or_date_string> <month> <day> [format <format_pattern>] [language <language_code>])
Return: date type. This function has two overloads: if <year_or_date_string> is an integer year, generate a date using <year_or_date_string>, <month>, <day>; if it is a string date, generate a date from the string.
Parameter **<year_or_date_string>**: integer year or string date. Required parameter; type is integer or string; parameter name omitted.
> Example: Generate date from string "2026-04-01".
NLC snippet: date("2026-04-01") // result is date 2026-04-01
Parameter **<month>**: month in the date. Non-required parameter; type is integer; parameter name omitted.
Parameter **<day>**: day in the date. Non-required parameter; type is integer; parameter name omitted.
> Example: Generate date from integer year 2026, month 4, day 1.
NLC snippet: date(2026,4,1) // result is date 2026-04-01
Parameter **[format <format_pattern>]**: When <year_or_date_string> is a string date, parse it using this pattern. Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Parse the string "04/01/2026" using pattern "MM/dd/yyyy".
NLC snippet: date("04/01/2026"; format "MM/dd/yyyy") // result is date 2026-04-01
Parameter **[language <language_code>]**: When <year_or_date_string> is a string date, parse it using this language and [format <format_pattern>]. Non-required parameter; type is string; parameter name cannot be omitted. Note: common languages include en (English) and zh (Chinese).
> Example: Parse "1 Apr 2026" using English pattern "d MMM yyyy".
NLC snippet: date("1 Apr 2026"; format "d MMM yyyy"; language "en") // result is date 2026-04-01