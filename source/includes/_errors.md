# Errors


The ReproConnect API uses the following HTTP error codes:


Code | Meaning
---------- | -------
400 | Bad Request -- Usually means your request is malformed, review the format of your request, especially the JSON payload for POST and PATCH requests
401 | Unauthorized -- Your API key is missing or invalid
403 | Forbidden -- You have hit your rate limit, or you are now allowed to access this endpoint or resource
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
500 | Internal Server Error -- We had a problem with our server. Try again later, or contact support if this persists.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
