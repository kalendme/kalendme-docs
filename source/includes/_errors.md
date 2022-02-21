# Errors

KalendMe's API can reply with the following error codes:

Error Code | Message | HTTP Status Code 
---------- | ------- | ---------------- 
1000 | Missing required body parameter | 400
1001 | Missing required query parameter | 400
1002 | Conflict | 409
1003 | The specified `timeZone` is invalid | 400
1004 | The specified `weekAvailaibility` is invalid. Must contain all 7 days of the week in the required format: start/end keys in 24-hour HH:MM. | 400
1005 | The specified `language` is invalid. We only support (en, es, pt). | 400
1006 | The specified `durationMinutes` is invalid. Can only be integers between 1 and 480. | 400
1007 | The specified `minimumNoticeMinutes` is invalid. Can only be integers between 0 and 90000 (~2 months). | 400
1008 | The `enabled` attribute must be a valid boolean. | 400
1009 | The `visible` attribute must be a valid boolean. | 400
1010 | The specified `title` is invalid. Can only be a string between 1 and 30 characters. | 400
1011 | The specified `description` is invalid. Can only be a string up to 60 characters. | 400
1012 | The specified `customQuestions` are invalid. Must be an array with { id, order, question } objects. | 400
1013 | You can't change a user's email. | 403
1014 | The specified `name` is invalid. Can only be a string up to 50 characters. | 400
1015 | The specified `urlString` is invalid, it can only contain letters, numbers, periods or hyphens (-) without spaces, a minimum of 1 character and a maximum of 30 characters. | 400
1016 | The specified `email` is invalid. Has to be a properly formatted email. | 400
1017 | The specified `year` is invalid. Must be a positive small integer. | 400
1018 | The specified `month` is invalid. Must be a positive integer between 1 and 12. | 400
1019 | The specified `selectedDateTime` is invalid. Must be a future unix timestamp. | 400
1020 | The specified `selectedDateTime` is not available anymore. There's a conflict. | 409
1021 | The specified `guest` array is invalid. Must be an non-empty array with valid email strings. | 400
1022 | The specified `mainGuestName` is invalid. Can only be a string up to 50 characters. | 400
1023 | The specified `mainGuestTimeZone` is invalid. | 400
1024 | The specified `providerId` is invalid. Can only be google or microsoft | 400
1025 | The specified `day` is invalid. Must be a real year-month-day combination. | 400
1026 | The specified `nickname` is invalid. Can only be a string up to 50 characters. | 400
1027 | The specified boolean `enabled` is invalid. Can only be a string equaling true or false. | 400
1028 | The specified `enabledEvents` list is invalid. Must contain at least one valid webhook event type lowercased. | 400
1029 | The specified `url` is invalid. Must be a valid URL starting with http/https. localhost is not allowed. | 400
1030 | The specified `status` is invalid. Must be a string that's either CONFIRMED or CANCELLED. | 400
1031 | The specified `location` is invalid. Can only be a string up to 256 characters. | 400