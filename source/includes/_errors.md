# Errors


The ReproConnect API uses the following HTTP error codes:


Code | Meaning
---------- | -------
400 | Bad Request -- Usually means your request is malformed, review the format of your request, especially the JSON payload for POST and PATCH requests
403 | Forbidden -- Your API key is invalid, or you've attempted to access a restricted resource
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
429 | Too Many Requests -- Your requests are being throttled, slow down
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
