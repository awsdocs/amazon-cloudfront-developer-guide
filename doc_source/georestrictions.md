# Restricting the geographic distribution of your content<a name="georestrictions"></a>

You can use *geographic restrictions*, sometimes known as *geo blocking*, to prevent users in specific geographic locations from accessing content that you're distributing through a CloudFront distribution\. To use geographic restrictions, you have two options:
+ Use the CloudFront geographic restrictions feature\. Use this option to restrict access to all of the files that are associated with a distribution and to restrict access at the country level\.
+ Use a third\-party geolocation service\. Use this option to restrict access to a subset of the files that are associated with a distribution or to restrict access at a finer granularity than the country level\.

**Topics**
+ [Using CloudFront geographic restrictions](#georestrictions-cloudfront)
+ [Using a third\-party geolocation service](#georestrictions-geolocation-service)

## Using CloudFront geographic restrictions<a name="georestrictions-cloudfront"></a>

When a user requests your content, CloudFront typically serves the requested content regardless of where the user is located\. If you need to prevent users in specific countries from accessing your content, you can use the CloudFront geographic restrictions feature to do one of the following:
+ Allow your users to access your content only if they’re in one of the approved countries on your allow list\.
+ Prevent your users from accessing your content if they’re in one of the banned countries on your block list\.

For example, if a request comes from a country where you are not authorized to distribute your content, you can use CloudFront geographic restrictions to block the request\.

**Note**  
CloudFront determines the location of your users by using a third\-party database\. The accuracy of the mapping between IP addresses and countries varies by Region\. Based on recent tests, the overall accuracy is 99\.8%\. If CloudFront can’t determine a user’s location, CloudFront serves the content that the user has requested\.

Here’s how geographic restrictions work:

1. Suppose you have rights to distribute your content only in Liechtenstein\. You update your CloudFront distribution to add an allow list that contains only Liechtenstein\. \(Alternatively, you could add a block list that contains every country except Liechtenstein\.\)

1. A user in Monaco requests your content, and DNS routes the request to a CloudFront edge location in Milan, Italy\.

1. The edge location in Milan looks up your distribution and determines that the user in Monaco is not allowed to download your content\.

1. CloudFront returns an HTTP status code `403 (Forbidden)` to the user\.

You can optionally configure CloudFront to return a custom error message to the user, and you can specify how long you want CloudFront to cache the error response for the requested file\. The default value is 10 seconds\. For more information, see [Creating a custom error page for specific HTTP status codes](GeneratingCustomErrorResponses.md#creating-custom-error-pages)\.

Geographic restrictions apply to an entire distribution\. If you need to apply one restriction to part of your content and a different restriction \(or no restriction\) to another part of your content, you must either create separate CloudFront distributions or [use a third\-party geolocation service](#georestrictions-geolocation-service)\.

If you enable CloudFront [standard logs](AccessLogs.md) \(access logs\), you can identify the requests that CloudFront rejected by searching for the log entries in which the value of `sc-status` \(the HTTP status code\) is `403`\. However, using only the standard logs, you can’t distinguish a request that CloudFront rejected based on the location of the user from a request that CloudFront rejected because the user didn’t have permission to access the file for another reason\. If you have a third\-party geolocation service such as Digital Element or MaxMind, you can identify the location of requests based on the IP address in the `c-ip` \(client IP\) column in the access logs\. For more information about CloudFront standard logs, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\.

The following procedure explains how to use the CloudFront console to add geographic restrictions to an existing distribution\. For information about how to use the console to create a distribution, see [Creating a distribution](distribution-web-creating-console.md)\.<a name="restrictions-geo-procedure"></a>

**To add geographic restrictions to your CloudFront web distribution \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose the distribution that you want to update\.

1. Choose **Geographic restrictions**\.

1. Choose **Edit**\.

1. Select **Allow list** to create a list of allowed countries, or **Block list** to create a list of blocked countries\.

1. Add the desired countries to the list, then choose **Save changes**\.

## Using a third\-party geolocation service<a name="georestrictions-geolocation-service"></a>

The CloudFront geographic restrictions feature lets you control distribution of your content at the country level for all files that you're distributing with a given web distribution\. If you have a use case for geographic restrictions where the restrictions don't follow country boundaries, or if you want to restrict access to only some of the files that you're serving by a given distribution, you can combine CloudFront with a third\-party geolocation service\. This can allow you to control access to your content based not only on country but also based on city, zip or postal code, or even latitude and longitude\.

When you're using a third\-party geolocation service, we recommend that you use CloudFront signed URLs, which let you specify an expiration date and time after which the URL is no longer valid\. In addition, we recommend that you use an Amazon S3 bucket as your origin because you can then use a CloudFront [origin access control](private-content-restricting-access-to-s3.md) to prevent users from accessing your content directly from the origin\. For more information about signed URLs and origin access control, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

The following steps explain how to control access to your files by using a third\-party geolocation service\.

**To use a third\-party geolocation service to restrict access to files in a CloudFront distribution**

1. Get an account with a geolocation service\.

1. Upload your content to an Amazon S3 bucket\.

1. Configure Amazon CloudFront and Amazon S3 to serve private content\. For more information, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

1. Write your web application to do the following:
   + Send the IP address for each user request to the geolocation service\.
   + Evaluate the return value from the geolocation service to determine whether the user is in a location where you want CloudFront to distribute your content\.
   + If you want to distribute your content to the user’s location, generate a signed URL for your CloudFront content\. If you don’t want to distribute content to that location, return HTTP status code `403 (Forbidden)` to the user\. Alternatively, you can configure CloudFront to return a custom error message\. For more information, see [Creating a custom error page for specific HTTP status codes](GeneratingCustomErrorResponses.md#creating-custom-error-pages)\.

   For more information, refer to the documentation for the geolocation service that you’re using\.

You can use a web server variable to get the IP addresses of the users who are visiting your website\. Note the following caveats:
+ If your web server is not connected to the internet through a load balancer, you can use a web server variable to get the remote IP address\. However, this IP address isn’t always the user’s IP address\. It can also be the IP address of a proxy server, depending on how the user is connected to the internet\.
+ If your web server is connected to the internet through a load balancer, a web server variable might contain the IP address of the load balancer, not the IP address of the user\. In this configuration, we recommend that you use the last IP address in the `X-Forwarded-For` HTTP header\. This header typically contains more than one IP address, most of which are for proxies or load balancers\. The last IP address in the list is the one most likely to be associated with the user’s geographic location\.

If your web server is not connected to a load balancer, we recommend that you use web server variables instead of the `X-Forwarded-For` header to avoid IP address spoofing\.