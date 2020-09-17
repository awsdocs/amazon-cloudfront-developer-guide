# Creating a Custom Error Page for Specific HTTP Status Codes<a name="custom-error-pages"></a>

If you'd rather display a custom error message instead of the default message—for example, a page that uses the same formatting as the rest of your website—you can have CloudFront return to the viewer an object \(such as an HTML file\) that contains your custom error message\.

To specify the specific file that you want to return and the errors for which the file should be returned, you update your CloudFront distribution to specify those values\. For more information, see [Custom Error Pages and Error Caching](distribution-web-values-specify.md#DownloadDistValuesErrorPages) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. 

For example, the following is a customized error message:

![\[AWS 404 page\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/custom-error-page-aws-404-example.png)

You can specify a different object for each supported HTTP status code, or you can use the same object for all of the supported status codes\. You can also choose to specify objects for some status codes and not for others\. 

The objects that you're serving through CloudFront can be unavailable for a variety of reasons\. These fall into two broad categories:
+ **Client errors** indicate a problem with the request\. For example, an object with the specified name isn't available, or the user doesn't have the permissions required to get an object in your Amazon S3 bucket\. When a client error occurs, the origin returns an HTTP status code in the 400 range to CloudFront\.
+ **Server errors** indicate a problem with the origin server\. For example, the HTTP server is busy or unavailable\. When a server error occurs, either your origin server returns an HTTP status code in the 500 range to CloudFront, or CloudFront doesn't get a response from your origin server for a certain period of time and assumes a 504 status code \(gateway timeout\)\.

The HTTP status codes for which CloudFront can return a custom error page include the following:
+ 400, 403, 404, 405, 414, 416
+ 500, 501, 502, 503, 504

**Note**  
You can create a custom error page for HTTP status code 416 \(Requested Range Not Satisfiable\), and you can change the HTTP status code that CloudFront returns to viewers when your origin returns a status code 416 to CloudFront\. \(For more information, see [Changing Response Codes Returned by CloudFront](custom-error-pages-response-code.md)\.\) However, CloudFront doesn't cache status code 416 responses, so you can specify a value for **Error Caching Minimum TTL** for status code 416, but CloudFront doesn't use it\. 

For a detailed explanation of how CloudFront handles error responses from your origin, see [How CloudFront Processes and Caches HTTP 4xx and 5xx Status Codes from Your Origin](HTTPStatusCodes.md)\.