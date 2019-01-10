# Configuring Error Response Behavior<a name="custom-error-pages-procedure"></a>

You can use either the CloudFront API or console to configure CloudFront error responses\. For information about using the CloudFront API to configure error responses, go to [PUT Distribution Config](https://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*, and see the `CustomErrorResponses` element\. 

**To configure CloudFront error responses using the console**

1. Create the custom error pages that you want CloudFront to return to viewers when your origin returns HTTP 4xx or 5xx errors\. Save the pages in a location that is accessible to CloudFront\. 

   We recommend that you store custom error pages in an Amazon S3 bucket even if you're using a custom origin\. If you store custom error pages on an HTTP server and the server starts to return 5xx errors, CloudFront can't get the files that you want to return to viewers because the origin server is unavailable\.

1. Confirm that you have granted CloudFront at least `read` permission to your custom error page objects\.

   For more information about Amazon S3 permissions, see [Access Control](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAuthAccess.html) in the *Amazon Simple Storage Service Developer Guide*\. For information on using the Amazon S3 console to update permissions, go to the [http://docs.aws.amazon.com/AmazonS3/latest/UG/Welcome.html](http://docs.aws.amazon.com/AmazonS3/latest/UG/Welcome.html)\. 

1. \(Optional\) Configure your origin server to add `Cache-Control` directives or an `Expires` header along with the error response for specific objects, if applicable\. For more information, see [Controlling How Long CloudFront Caches Errors](custom-error-pages-expiration.md)\.

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the list of distributions, select the distribution to update and choose **Distribution Settings**\.

1. Choose the **Error Pages** tab\. Then either choose **Create Custom Error Response**, or choose an existing error code and choose **Edit**\.  
![\[Select Web as the distribution type.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

1. Enter the applicable values\. For more information, see [Custom Error Pages and Error Caching](distribution-web-values-specify.md#DownloadDistValuesErrorPages)\.

1. If you configured CloudFront to return custom error pages, add or update the applicable cache behaviors\. For more information, see [Storing Objects and Custom Error Pages in Different Locations](custom-error-pages-cache-behavior.md)\.

1. To save your changes, choose **Yes, Edit**\.