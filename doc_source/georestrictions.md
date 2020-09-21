# Restricting the Geographic Distribution of Your Content<a name="georestrictions"></a>

You can use *geo restriction*, also known as *geo blocking*, to prevent users in specific geographic locations from accessing content that you're distributing through a CloudFront web distribution\. To use geo restriction, you have two options:
+ Use the CloudFront geo restriction feature\. Use this option to restrict access to all of the files that are associated with a distribution and to restrict access at the country level\. 
+ Use a third\-party geolocation service\. Use this option to restrict access to a subset of the files that are associated with a distribution or to restrict access at a finer granularity than the country level\.

**Topics**
+ [Using CloudFront Geo Restriction](#georestrictions-cloudfront)
+ [Using a Third\-Party Geolocation Service](#georestrictions-geolocation-service)

## Using CloudFront Geo Restriction<a name="georestrictions-cloudfront"></a>

When a user requests your content, CloudFront typically serves the requested content regardless of where the user is located\. If you need to prevent users in specific countries from accessing your content, you can use the CloudFront geo restriction feature to do one of the following:
+ Allow your users to access your content only if they're in one of the countries on a whitelist of approved countries\. 
+ Prevent your users from accessing your content if they're in one of the countries on a blacklist of banned countries\. 

For example, if a request comes from a country where, for copyright reasons, you are not authorized to distribute your content, you can use CloudFront geo restriction to block the request\.

**Note**  
CloudFront determines the location of your users by using a third\-party GeoIP database\. The accuracy of the mapping between IP addresses and countries varies by Region\. Based on recent tests, the overall accuracy is 99\.8%\. If CloudFront can't determine a user's location, CloudFront serves the content that the user has requested\.

Here's how geo restriction works:

1. Suppose you have rights to distribute your content only in Liechtenstein\. You update your CloudFront web distribution and add a whitelist that contains only Liechtenstein\. \(Alternatively, you could add a blacklist that contains every country except Liechtenstein\.\)

1. A user in Monaco requests your content, and DNS routes the request to the CloudFront edge location in Milan, Italy\.

1. The edge location in Milan looks up your distribution and determines that the user in Monaco is not allowed to download your content\.

1. CloudFront returns an HTTP status code `403 (Forbidden)` to the user\.

You can optionally configure CloudFront to return a custom error message to the user, and you can specify how long you want CloudFront to cache the error response for the requested file\. The default value is 10 seconds\. For more information, see [Creating a Custom Error Page for Specific HTTP Status Codes](custom-error-pages.md)\.

Geo restriction applies to an entire web distribution\. If you need to apply one restriction to part of your content and a different restriction \(or no restriction\) to another part of your content, you must either create separate CloudFront web distributions or use a third\-party geolocation service\.

If you enable CloudFront access logging, you can identify the requests that CloudFront rejected by searching for the log entries for which the value of `sc-status` \(the HTTP status code\) is `403`\. However, using only the access logs, you can't distinguish a request that CloudFront rejected based on the location of the user from a request that CloudFront rejected because the user didn't have permission to access the file for another reason\. If you have a third\-party geolocation service such as Digital Element or MaxMind, you can identify the location of requests based on the IP address in the `c-ip` \(client IP\) column in the access logs\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

The following procedure explains how to use the CloudFront console to add geo restriction to an existing web distribution\. For information about how to use the console to create a web distribution, see [Creating a Distribution](distribution-web-creating-console.md)\.<a name="restrictions-geo-procedure"></a>

**To add geo restriction to your CloudFront web distribution \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the distribution that you want to update\.

1. In the **Distribution Settings** pane, choose the **Restrictions** tab\.

1. Choose **Edit**\.

1. Enter the applicable values\. For more information, see [Restrictions](distribution-web-values-specify.md#DownloadDistValuesRestrictions)\.

1. Choose **Yes, Edit**\.

## Using a Third\-Party Geolocation Service<a name="georestrictions-geolocation-service"></a>

The CloudFront geo restriction feature lets you control distribution of your content at the country level for all files that you're distributing with a given web distribution\. If you have geographic restrictions on where your content can be distributed and the restrictions don't follow country boundaries, or if you want to restrict access to only some of the files that you're distributing through CloudFront, you can combine CloudFront with a third\-party geolocation service\. This can allow you to control access to your content based not only on country but also based on city, zip or postal code, or even latitude and longitude\.

When you're using a third\-party geolocation service, we recommend that you use CloudFront signed URLs, which let you specify an expiration date and time after which the URL is no longer valid\. In addition, we recommend that you use an Amazon S3 bucket as your origin because you can then use a CloudFront origin access identity to prevent users from accessing your content directly from the origin\. For more information about signed URLs and origin access identities, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

The following steps explain how to control access to your files by using a third\-party geolocation service\. 

**To use geographic location to restrict access to files in a CloudFront distribution**

1. Get an account with a geolocation service\.

1. Upload your content to an Amazon Simple Storage Service \(S3\) bucket\. For more information, see the [Amazon S3 documentation](http://aws.amazon.com/documentation/s3/)\. 

1. Configure Amazon CloudFront and Amazon S3 to serve private content\. For more information, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

1. Write your web application to do the following:

   1. Send the IP address for each user request to the geolocation service\.

   1. Evaluate the return value from the geolocation service to determine whether the user is in a location to which you want CloudFront to distribute your content\.

   1. If you want to distribute your content to the user's location, generate a signed URL for your CloudFront content\. If you don't want to distribute content to that location, return HTTP status code `403 (Forbidden)` to the user\. Alternatively, you can configure CloudFront to return a custom error message\. For more information, see [Creating a Custom Error Page for Specific HTTP Status Codes](custom-error-pages.md)\.

   For more information, refer to the documentation for the geolocation service that you're using\.

You can use a web server variable to get the IP addresses of the users who are visiting your website\. Note the following caveats:
+ If your web server is not connected to the internet through a load balancer, you can use a web server variable to get the remote IP address\. However, this IP address isn't always the user's IP address\. It can also be the IP address of a proxy server, depending on how the user is connected to the internet\.
+ If your web server is connected to the internet through a load balancer, a web server variable might contain the IP address of the load balancer, not the IP address of the user\. In this configuration, we recommend that you use the last IP address in the `X-Forwarded-For` HTTP header\. This header typically contains more than one IP address, most of which are for proxies or load balancers\. The last IP address in the list is the one most likely to be associated with the user's geographic location\.

If your web server is not connected to a load balancer, we recommend that you use web server variables instead of the `X-Forwarded-For` header to avoid IP address spoofing\. 