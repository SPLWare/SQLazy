#### function: time
Syntax: time(<hour_or_time_string> <minute> <second> [format <format_pattern>] [language <language_code>] [precise])
Return: time type. This function has two overloads: if <hour_or_time_string> is an integer hour, generate a time using <hour_or_time_string>, <minute>, <second>; if it is a string time, generate a time from the string.
Parameter **<hour_or_time_string>**: integer hour or string time. Required parameter; type is integer or string; parameter name omitted.
> Example: Generate time from string "08:30:50".
NLC snippet: time("08:30:50") // result is time 08:30:50
Parameter **<minute>**: minute in the time. Non-required parameter; type is integer; parameter name omitted.
Parameter **<second>**: second in the time. Non-required parameter; type is integer; parameter name omitted.
> Example: Generate time from integer hour 8, minute 30, second 50.
NLC snippet: time(8,30,50) // result is time 08:30:50
Parameter **[format <format_pattern>]**: When <hour_or_time_string> is a string time, parse it using this pattern. Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Parse "08/30/50" using pattern "HH/mm/ss".
NLC snippet: time("08/30/50"; format "HH/mm/ss") // result is time 08:30:50
Parameter **[language <language_code>]**: When <hour_or_time_string> is a string time, parse it using this language and [format <format_pattern>]. Non-required parameter; type is string; parameter name cannot be omitted. Note: common languages include en and zh.
> Example: Parse "8:30 AM" using English pattern "h:mm a".
NLC snippet: time("8:30 AM"; format "h:mm a"; language "en") // result is time 08:30:00
Parameter **[precise]**: When converting the string <hour_or_time_string> to time, restrict precision to hour, minute, or second. Non-required parameter; type is enumeration; enumeration values: hour, minute, second; parameter name cannot be omitted.
> Example: Convert the string "08:30:50:640" to time with minute precision.
NLC snippet: time(time("08:30:50:640"); precise minute) // result is time 08:30:00