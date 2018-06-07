# How CloudFront Processes HTTP and HTTPS Requests<a name="HTTPandHTTPSRequests"></a>

For Amazon S3 origins, CloudFront accepts requests in both HTTP and HTTPS protocols for objects in a CloudFront distribution by default\. CloudFront then forwards the requests to your Amazon S3 bucket using the same protocol in which the requests were made\. 

For custom origins, when you create your distribution, you can specify how CloudFront accesses your origin: HTTP only, or matching the protocol that is used by the viewer\. For more information about how CloudFront handles HTTP and HTTPS requests for custom origins, see [Protocols](RequestAndResponseBehaviorCustomOrigin.md#RequestCustomProtocols)\.

For information about how to restrict your web distribution so that end users can only access objects using HTTPS, see [Using HTTPS with CloudFront](using-https.md)\. \(This option doesn't apply to RTMP distributions, which use the RTMP protocol\.\)

**Note**  
The charge for HTTPS requests is higher than the charge for HTTP requests\. For more information about billing rates, go to the [CloudFront pricing plan](http://aws.amazon.com/cloudfront/#pricing)\.