# HTTP Status Codes

The Feathery API uses the following codes:


Status Code | Meaning
---------- | -------
200 | OK -- Everything worked as expected.
201 | Created -- Everything worked as expected. A resource was created.
204 | No Content -- Everything worked as expected. No content was returned.
400 | Bad Request -- The request was unacceptable, often due to missing a required parameter.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The API key doesn't have permissions to perform the request.
404 | Not Found -- The requested resource doesn't exist.
405 | Method Not Allowed -- You specified an invalid method on the requestedURL.
429 | Too Many Requests -- Too many requests hit the API too quickly. We recommend an exponential backoff of your requests.
500 | Something went wrong on Feathery's end. (These are rare.)
