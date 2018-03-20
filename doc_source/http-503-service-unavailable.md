# HTTP 503 Status Code \(Service Unavailable\)<a name="http-503-service-unavailable"></a>

An HTTP 503 status code \(Service Unavailable\) typically indicates a performance issue on the origin server\. In rare cases, it indicates that CloudFront temporarily can't satisfy a request because of limited resources at an edge location\.

**Topics**
+ [Origin Server Does Not Have Enough Capacity to Support the Request Rate](#http-503-service-unavailable-not-enough-origin-capacity)
+ [CloudFront Was Not Able to Resolve Your Origin Domain Due to DNS Issues](#http-503-service-unavailable-origin-domain-dns-issues)
+ [CloudFront Caused the Error Due to Limited Resources at the Edge Location](#http-503-service-unavailable-limited-resources-at-edge-location)

## Origin Server Does Not Have Enough Capacity to Support the Request Rate<a name="http-503-service-unavailable-not-enough-origin-capacity"></a>

CloudFront generates this error when the origin server is overwhelmed with incoming requests\. CloudFront then relays the error back to the user\. To resolve this issue, try the following solutions:
+ If you use Amazon S3 as your origin server, optimize the performance of Amazon S3 by following the best practices for key naming\. For more information, see [Request Rate and Performance Considerations](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html) in the *Amazon Simple Storage Service Developer Guide*\.
+ If you use Elastic Load Balancing as your origin server, see [503 Error Classic](https://aws.amazon.com/premiumsupport/knowledge-center/503-error-classic/)\.
+ If you use a custom origin, examine the application logs to ensure that your origin has sufficient resources, such as memory, CPU, and disk size\. If you use Amazon EC2 as the backend, make sure that the instance type has the appropriate resources to fulfill the incoming requests\. For more information, see [Instance Types](http://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/instance-types.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## CloudFront Was Not Able to Resolve Your Origin Domain Due to DNS Issues<a name="http-503-service-unavailable-origin-domain-dns-issues"></a>

When CloudFront receives a request for an object that is expired or is not stored in its cache, it makes a request to the origin to get the updated object\. To make a successful request to the origin, CloudFront performs a DNS resolution on the origin domain name\. However, when the DNS service that hosts your domain is experiencing issues, CloudFront cannot resolve the domain name to get the IP address, resulting in a 503 error\. To fix this issue, contact your DNS provider, or, if you are using Amazon Route 53, see [Amazon Route 53 DNS](https://aws.amazon.com/premiumsupport/knowledge-center/route-53-dns-website-unreachable/)\.

To further troubleshoot this issue, ensure that the [authoritative name servers](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-authoritative-name-server) of your origin's root domain or zone apex \(such as `example.com`\) are functioning correctly\. Your authoritative name servers then receive the request and return the IP address that is associated with the domain, and are the same as the DNS servers that you used to set up your CloudFront distribution\. Use the following commands to find the name servers for your apex origin:

```
dig OriginAPEXDomainName NS +short 
nslookup –query=NS OriginAPEXDomainName
```

When you have the names of your name servers, use the following commands to query the domain name of your origin against them to make sure that each responds with an answer:

```
dig OriginDomainName @NameServerFromAbove
nslookup OriginDomainName NameServerFromAbove
```

## CloudFront Caused the Error Due to Limited Resources at the Edge Location<a name="http-503-service-unavailable-limited-resources-at-edge-location"></a>

You will receive this error in the rare case that CloudFront has the ability to route requests to the next best available edge location\. In rare cases, CloudFront might be unable to satisfy the request\. If this happens, contact [AWS Support](https://console.aws.amazon.com/support/home)\.

This error is common when you perform load testing on your CloudFront distribution\. To avoid this, follow the [Load Testing CloudFront](load-testing.md) guidelines to avoid 503 \(Capacity Exceeded\) errors\. This type of error typically means that the edge location didn't have enough capacity at the time of the request to serve the object\.