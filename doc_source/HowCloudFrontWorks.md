# How CloudFront delivers content<a name="HowCloudFrontWorks"></a>

After some initial setup, CloudFront works together with your website or application and speeds up delivery of your content\. This section explains how CloudFront serves your content when viewers request it\. 

**Topics**
+ [How CloudFront delivers content to your users](#HowCloudFrontWorksContentDelivery)
+ [How CloudFront works with regional edge caches](#CloudFrontRegionaledgecaches)

## How CloudFront delivers content to your users<a name="HowCloudFrontWorksContentDelivery"></a>

After you configure CloudFront to deliver your content, here's what happens when users request your files:

1. A user accesses your website or application and requests one or more files, such as an image file and an HTML file\.

1. DNS routes the request to the CloudFront POP \(edge location\) that can best serve the request—typically the nearest CloudFront POP in terms of latency—and routes the request to that edge location\. 

1. In the POP, CloudFront checks its cache for the requested files\. If the files are in the cache, CloudFront returns them to the user\. If the files are *not* in the cache, it does the following:

   1. CloudFront compares the request with the specifications in your distribution and forwards the request for the files to your origin server for the corresponding file type—for example, to your Amazon S3 bucket for image files and to your HTTP server for HTML files\. 

   1. The origin servers send the files back to the edge location\.

   1. As soon as the first byte arrives from the origin, CloudFront begins to forward the files to the user\. CloudFront also adds the files to the cache in the edge location for the next time someone requests those files\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/how-cloudfront-delivers-content.png)

## How CloudFront works with regional edge caches<a name="CloudFrontRegionaledgecaches"></a>

CloudFront points of presence \(POPs\) \(edge locations\) make sure that popular content can be served quickly to your viewers\. CloudFront also has *regional edge caches* that bring more of your content closer to your viewers, even when the content is not popular enough to stay at a POP, to help improve performance for that content\.

Regional edge caches help with all types of content, particularly content that tends to become less popular over time\. Examples include user\-generated content, such as video, photos, or artwork; e\-commerce assets such as product photos and videos; and news and event\-related content that might suddenly find new popularity\.

**How regional caches work**  
Regional edge caches are CloudFront locations that are deployed globally, close to your viewers\. They're located between your origin server and the POPs—global edge locations that serve content directly to viewers\. As objects become less popular, individual POPs might remove those objects to make room for more popular content\. Regional edge caches have a larger cache than an individual POP, so objects remain in the cache longer at the nearest regional edge cache location\. This helps keep more of your content closer to your viewers, reducing the need for CloudFront to go back to your origin server, and improving overall performance for viewers\. 

When a viewer makes a request on your website or through your application, DNS routes the request to the POP that can best serve the user’s request\. This location is typically the nearest CloudFront edge location in terms of latency\. In the POP, CloudFront checks its cache for the requested files\. If the files are in the cache, CloudFront returns them to the user\. If the files are not in the cache, the POPs go to the nearest regional edge cache to fetch the object\. 

In the regional edge cache location, CloudFront again checks its cache for the requested files\. If the files are in the cache, CloudFront forwards the files to the POP that requested them\. As soon as the first byte arrives from regional edge cache location, CloudFront begins to forward the files to the user\. CloudFront also adds the files to the cache in the POP for the next time someone requests those files\. 

For files not cached at either the POP or the regional edge cache location, CloudFront compares the request with the specifications in your distributions and forwards the request for your files to the origin server\. After your origin server sends the files back to the regional edge cache location, they are forwarded to the POP, and CloudFront forwards the files to the user\. In this case, CloudFront also adds the files to the cache in the regional edge cache location in addition to the POP for the next time a viewer requests those files\. This makes sure that all of the POPs in a region share a local cache, eliminating multiple requests to origin servers\. CloudFront also keeps persistent connections with origin servers so files are fetched from the origins as quickly as possible\.

**Note**  
Regional edge caches have feature parity with POPs\. For example, a cache invalidation request removes an object from both POP caches and regional edge caches before it expires\. The next time a viewer requests the object, CloudFront returns to the origin to fetch the latest version of the object\.
Proxy methods `PUT/POST/PATCH/OPTIONS/DELETE` go directly to the origin from the POPs and do not proxy through the regional edge caches\.
Dynamic requests, as determined at request time, do not flow through regional edge caches, but go directly to the origin\.