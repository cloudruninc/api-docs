# Errors

The Cloudrun API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is incorrect.
401 | Unauthorized -- Your API token is wrong.
403 | Forbidden -- The resource requested is hidden for administrators only.
404 | Not Found -- The specified resource could not be found.
429 | Too Many Requests -- Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
