# Requirements and Recommendations for Using Amazon EC2 and Other Custom Origins<a name="CustomOriginBestPractices"></a>

Follow these guidelines for using Amazon EC2 instances and other custom origins with CloudFront\. 
+ Host and serve the same content on all servers\.
+ Log the `X-Amz-Cf-Id` header entries on all servers; CloudFront requires this information for debugging\.
+ Restrict access requests to the HTTP and HTTPS ports that your custom origin listens on\.
+ Synchronize the clocks of all servers in your implementation\.
+ Use redundant servers to handle failures\.
+ For information about using a custom origin to serve private content, see [Using an HTTP Server for Private Content](PrivateContent.md#private-content-overview-choosing-origin)\.
+ For information about request and response behavior and about supported HTTP status codes, see [Request and Response Behavior](RequestAndResponseBehavior.md)\.

If you use Amazon Elastic Compute Cloud for your custom origins, we recommend that you do the following:

1. Use an Amazon Machine Image that automatically installs the software for a web server\. For more information, see the [Amazon EC2 documentation](http://aws.amazon.com/documentation/ec2/)\.

1. Use an Elastic Load Balancing load balancer to handle traffic across multiple Amazon EC2 instances and to isolate your application from changes to Amazon EC2 instances\. For example, if you use a load balancer, you can add and delete Amazon EC2 instances without changing your application\. For more information, see the [Elastic Load Balancing documentation](http://aws.amazon.com/documentation/elasticloadbalancing/)\.

1. When you create your CloudFront distribution, specify the URL of the load balancer for the domain name of your origin server\. For more information, see [Working with Web Distributions](distribution-web.md)\.