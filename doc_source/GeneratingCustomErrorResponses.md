# Generating Custom Error Responses<a name="GeneratingCustomErrorResponses"></a>

If the objects that you're serving through CloudFront are unavailable for some reason, your web server typically returns an HTTP status code to CloudFront\. For example, if a viewer specifies an invalid URL, your web server returns a 404 status code to CloudFront, and CloudFront returns that status code to the viewer\. The viewer displays a brief and sparsely formatted default message similar to this:

`Not Found: The requested URL /myfilename.html was not found on this server.`

But you can display a custom error message instead, if you like\. You also have several options for managing how CloudFront responds when there's an error\.

**Topics**
+ [Creating a Custom Error Page for Specific HTTP Status Codes](custom-error-pages.md)
+ [Creating or Updating a Cache Behavior for Custom Error Pages](custom-error-pages-cache-behavior.md)
+ [Changing Response Codes](custom-error-pages-response-code.md)
+ [Controlling How Long CloudFront Caches Errors](custom-error-pages-expiration.md)
+ [How CloudFront Responds When a Custom Error Page Is Unavailable](custom-error-pages-unavailable.md)
+ [Pricing for Custom Error Pages](custom-error-pages-charges.md)
+ [Configuring Error Response Behavior](custom-error-pages-procedure.md)