# Updating HTTP Responses in Origin\-Response Triggers<a name="lambda-updating-http-responses"></a>

When CloudFront receives an HTTP response from the origin server, if there is an origin\-response trigger associated with the cache behavior, you can modify the HTTP response to override what was returned from the origin\.

Some common scenarios for updating HTTP responses include the following:
+ Changing the status to set an HTTP 200 status code and creating static body content to return to the viewer when an origin returns an error status code \(4xx or 5xx\)\. For sample code, see [Example: Using an Origin\-Response Trigger to Update the Error Status Code to 200\-OK](lambda-examples.md#lambda-examples-custom-error-static-body)\.
+ Changing the status to set an HTTP 301 or HTTP 302 status code, to redirect the user to another website when an origin returns an error status code \(4xx or 5xx\)\. For sample code, see [Example: Using an Origin\-Response Trigger to Update the Error Status Code to 302\-Found](lambda-examples.md#lambda-examples-custom-error-new-site)\.

You can also replace the HTTP responses in viewer and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses-in-requests.md)\.

When you’re working with the HTTP response, Lambda@Edge does not expose the HTML body that is returned by the origin server to the origin\-response trigger\. You can generate a static content body by setting it to the desired value, or remove the body inside the function by setting the value to be empty\. If you don’t update the body field in your function, the original body returned by the origin server is returned back to viewer\.