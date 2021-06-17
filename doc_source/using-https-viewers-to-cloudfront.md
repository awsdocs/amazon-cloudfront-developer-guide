# Requiring HTTPS for Communication Between Viewers and CloudFront<a name="using-https-viewers-to-cloudfront"></a>

You can configure one or more cache behaviors in your CloudFront distribution to require HTTPS for communication between viewers and CloudFront\. You also can configure one or more cache behaviors to allow both HTTP and HTTPS, so that CloudFront requires HTTPS for some objects but not for others\. The configuration steps depend on which domain name you're using in object URLs:
+ If you're using the domain name that CloudFront assigned to your distribution, such as d111111abcdef8\.cloudfront\.net, you change the **Viewer Protocol Policy** setting for one or more cache behaviors to require HTTPS communication\. In that configuration, CloudFront provides the SSL/TLS certificate\. 

  To change the value of **Viewer Protocol Policy** by using the CloudFront console, see the procedure later in this section\.

  For information about how to use the CloudFront API to change the value of the `ViewerProtocolPolicy` element, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.
+ If you're using your own domain name, such as example\.com, you need to change several CloudFront settings\. You also need to use an SSL/TLS certificate provided by AWS Certificate Manager \(ACM\), or import a certificate from a third\-party certificate authority into ACM or the IAM certificate store\. For more information, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

**Note**  
If you want to ensure that the objects that viewers get from CloudFront were encrypted when CloudFront got them from your origin, always use HTTPS between CloudFront and your origin\. If you recently changed from HTTP to HTTPS between CloudFront and your origin, we recommend that you invalidate objects in CloudFront edge locations\. CloudFront will return an object to a viewer regardless of whether the protocol used by the viewer \(HTTP or HTTPS\) matches the protocol that CloudFront used to get the object\. For more information about removing or replacing objects in a distribution, see [Adding, Removing, or Replacing Content That CloudFront Distributes](AddRemoveReplaceObjects.md)\.

To require HTTPS between viewers and CloudFront for one or more cache behaviors, perform the following procedure\.<a name="using-https-viewers-to-cloudfront-procedure"></a>

**To configure CloudFront to require HTTPS between viewers and CloudFront**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the top pane of the CloudFront console, choose the ID for the distribution that you want to update\.

1. On the **Behaviors** tab, choose the cache behavior that you want to update, and then choose **Edit**\.

1. Specify one of the following values for **Viewer Protocol Policy**:  
**Redirect HTTP to HTTPS**  
Viewers can use both protocols\. HTTP `GET` and `HEAD` requests are automatically redirected to HTTPS requests\. CloudFront returns HTTP status code 301 \(Moved Permanently\) along with the new HTTPS URL\. The viewer then resubmits the request to CloudFront using the HTTPS URL\.  
If you send `POST`, `PUT`, `DELETE`, `OPTIONS`, or `PATCH` over HTTP with an HTTP to HTTPS cache behavior and a request protocol version of HTTP 1\.1 or above, CloudFront redirects the request to a HTTPS location with a HTTP status code 307 \(Temporary Redirect\)\. This guarantees that the request is sent again to the new location using the same method and body payload\.  
If you send `POST`, `PUT`, `DELETE`, `OPTIONS`, or `PATCH` requests over HTTP to HTTPS cache behavior with a request protocol version below HTTP 1\.1, CloudFront returns a HTTP status code 403 \(Forbidden\)\.
When a viewer makes an HTTP request that is redirected to an HTTPS request, CloudFront charges for both requests\. For the HTTP request, the charge is only for the request and for the headers that CloudFront returns to the viewer\. For the HTTPS request, the charge is for the request, and for the headers and the object that are returned by your origin\.  
**HTTPS Only**  
Viewers can access your content only if they're using HTTPS\. If a viewer sends an HTTP request instead of an HTTPS request, CloudFront returns HTTP status code 403 \(Forbidden\) and does not return the object\.

1. Choose **Yes, Edit**\.

1. Repeat steps 3 through 5 for each additional cache behavior that you want to require HTTPS for between viewers and CloudFront\.

1. Confirm the following before you use the updated configuration in a production environment:
   + The path pattern in each cache behavior applies only to the requests that you want viewers to use HTTPS for\.
   + The cache behaviors are listed in the order that you want CloudFront to evaluate them in\. For more information, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.
   + The cache behaviors are routing requests to the correct origins\. 