# Interpreting Your AWS Bill and the AWS Usage Report for CloudFront<a name="billing-and-usage-interpreting"></a>

Your AWS bill for CloudFront service includes codes and abbreviations that might not be immediately obvious\. The first column in the following table lists items that appear in your bill and explains what each means\.

In addition, you can get an AWS usage report for CloudFront that contains more detail than the AWS bill for CloudFront\. The second column in the table lists items that appear in the usage report and shows the correlation between bill items and usage report items\.

Most codes in both columns include a two\-letter abbreviation that indicates the location of the activity\. In the following table, *region* in a code is replaced in your AWS bill and in the usage report by one of the following two\-letter abbreviations :
+ **AP:** Hong Kong, Philippines, South Korea, Taiwan, and Singapore \(Asia Pacific\)
+ **AU:** Australia
+ **CA:** Canada
+ **EU:** Europe and Israel
+ **IN:** India
+ **JP:** Japan
+ **ME:** Middle East
+ **SA:** South America
+ **US:** United States
+ **ZA:** South Africa

For more information about pricing by region, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

**Note**  
This table doesn't include charges for transferring your objects from an Amazon S3 bucket to CloudFront edge locations\. These charges, if any, appear in the **AWS Data Transfer** portion of your AWS bill\.


****  

| Items in Your CloudFront Bill | Values in the Usage Type Column in the CloudFront Usage Report | 
| --- | --- | 
|  *region***\-DataTransfer\-Out\-Bytes** Sum of bytes that CloudFront served for web and RTMP distributions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/billing-and-usage-interpreting.html)  |  **Web distributions:** [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/billing-and-usage-interpreting.html) **RTMP distributions:** [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/billing-and-usage-interpreting.html)  | 
|  *region***\-DataTransfer\-Out\-OBytes** **Web distributions only:** Total bytes transferred from CloudFront edge locations to your origin in response to `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\. The charges include data transfer for WebSocket data from client to server\.  |  *region***\-Out\-OBytes\-HTTP\-Proxy** Total bytes transferred via HTTP from CloudFront edge locations to your origin in response to `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\. *region***\-Out\-OBytes\-HTTPS\-Proxy** Total bytes transferred via HTTPS from CloudFront edge locations to your origin in response to `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\.  | 
|  *region***\-Requests\-Tier1** **Web distributions only:** Number of HTTP `GET` and `HEAD` requests  |  *region***\-Requests\-HTTP\-Static** Number of HTTP `GET` and `HEAD` requests served for objects with TTL ≥ 3600 seconds *region***\-Requests\-HTTP\-Dynamic** Number of HTTP `GET` and `HEAD` requests served for objects with TTL < 3600 seconds  | 
|  *region***\-Requests\-Tier2\-HTTPS** **Web distributions only:** Number of HTTPS `GET` and `HEAD` requests  |  *region***\-Requests\-HTTPS\-Static** Number of HTTPS `GET` and `HEAD` requests served for objects with TTL ≥ 3600 seconds *region***\-Requests\-HTTPS\-Dynamic** Number of HTTPS `GET` and `HEAD` requests served for objects with TTL < 3600 seconds  | 
|  *region***\-Requests\-HTTP\-Proxy** **Web distributions only:** Number of HTTP `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests that CloudFront forwards to your origin  |  *region***\-Requests\-HTTP\-Proxy** Same as the corresponding item in your CloudFront bill  | 
|  *region***\-Requests\-HTTPS\-Proxy** **Web distributions only:** Number of HTTPS `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests that CloudFront forwards to your origin  |  *region***\-Requests\-HTTPS\-Proxy** Same as the corresponding item in your CloudFront bill  | 
|  *region***\-Requests\-HTTPS\-Proxy\-FLE** **Web distributions only:** Number of HTTPS `DELETE`, `OPTIONS`, `PATCH`, and `POST` requests that CloudFront forwards to your origin which were processed with [field\-level encryption](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html)\.   |  *region***\-Requests\-HTTPS\-Proxy\-FLE** Same as the corresponding item in your CloudFront bill  | 
|  **Invalidations** **Web distributions only:** The charge for invalidating objects \(removing the objects from CloudFront edge locations\); for more information, see [Paying for File Invalidation](Invalidation.md#PayingForInvalidation)\.  |  **Invalidations** Same as the corresponding item in your CloudFront bill  | 
|  **SSL\-Cert\-Custom** **Web distributions only:** The charge for using an SSL certificate with a CloudFront alternate domain name such as example\.com instead of using the default CloudFront SSL certificate and the domain name that CloudFront assigned to your distribution\.  |  **SSL\-Cert\-Custom** Same as the corresponding item in your CloudFront bill  | 