# How CloudFront Delivers Content<a name="HowCloudFrontWorks"></a>

After some initial setup, CloudFront works invisibly to speed up delivery of your content\. This overview includes both the steps you perform before your first user accesses your application or website, and how CloudFront serves your content when viewers request it\. 

**Topics**
+ [Setting up CloudFront to Deliver Your Content](#HowCloudFrontWorksOverview)
+ [How CloudFront Delivers Content to Your Users](#HowCloudFrontWorksContentDelivery)
+ [How CloudFront Works with Regional Edge Caches](#CloudFrontRegionaledgecaches)

## Setting up CloudFront to Deliver Your Content<a name="HowCloudFrontWorksOverview"></a>

You create a CloudFront distribution to tell CloudFront where you want content to be delivered from, and the details about how to track and manage content delivery\. Then CloudFront uses computers—edge servers—that are close to your viewers to deliver that content quickly when someone wants to see it or use it\.<a name="HowCloudFrontWorksConfiguration"></a>

**How You Configure CloudFront to Deliver Your Content**

1. You specify *origin servers*, like an Amazon S3 bucket or your own HTTP server, from which CloudFront gets your files which will then be distributed from CloudFront edge locations all over the world\. 

   An origin server stores the original, definitive version of your objects\. If you're serving content over HTTP, your origin server is either an Amazon S3 bucket or an HTTP server, such as a web server\. Your HTTP server can run on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance or on a server that you manage; these servers are also known as *custom origins\.*

   If you use the Adobe Media Server RTMP protocol to distribute media files on demand, your origin server is always an Amazon S3 bucket\.

1. You upload your files to your origin servers\. Your files, also known as *objects*, typically include web pages, images, and media files, but can be anything that can be served over HTTP or a supported version of Adobe RTMP, the protocol used by Adobe Flash Media Server\.

   If you're using an Amazon S3 bucket as an origin server, you can make the objects in your bucket publicly readable, so that anyone who knows the CloudFront URLs for your objects can access them\. You also have the option of keeping objects private and controlling who accesses them\. See [Serving Private Content through CloudFront](PrivateContent.md)\. 

1. You create a CloudFront *distribution*, which tells CloudFront which origin servers to get your files from when users request the files through your web site or application\. At the same time, you specify details such as whether you want CloudFront to log all requests and whether you want the distribution to be enabled as soon as it's created\.

1. CloudFront assigns a domain name to your new distribution that you can see in the CloudFront console, or that is returned in the response to a programmatic request, for example, an API request\.

1. CloudFront sends your distribution's configuration \(but not your content\) to all of its **edge locations**—collections of servers in geographically dispersed data centers where CloudFront caches copies of your objects\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/how-you-configure-cf.png)

As you develop your website or application, you use the domain name that CloudFront provides for your URLs\. For example, if CloudFront returns `d111111abcdef8.cloudfront.net` as the domain name for your distribution, the URL for logo\.jpg in your Amazon S3 bucket \(or in the root directory on an HTTP server\) will be `http://d111111abcdef8.cloudfront.net/logo.jpg`\.

Or you can configure your CloudFront distribution so you can use your own domain name\. In that case, the URL might be `http://www.example.com/logo.jpg`\.

Optionally, you can configure your origin server to add headers to the files, to indicate how long you want the files to stay in the cache in CloudFront edge locations\. By default, each object stays in an edge location for 24 hours before it expires\. The minimum expiration time is 0 seconds; there isn't a maximum expiration time limit\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

## How CloudFront Delivers Content to Your Users<a name="HowCloudFrontWorksContentDelivery"></a>

After you configure CloudFront to deliver your content, here's what happens when users request your objects:

1. A user accesses your website or application and requests one or more objects, such as an image file and an HTML file\.

1. DNS routes the request to the CloudFront edge location that can best serve the request—typically the nearest CloudFront edge location in terms of latency—and routes the request to that edge location\. 

1. In the edge location, CloudFront checks its cache for the requested files\. If the files are in the cache, CloudFront returns them to the user\. If the files are *not* in the cache, it does the following:

   1. CloudFront compares the request with the specifications in your distribution and forwards the request for the files to the applicable origin server for the corresponding file type—for example, to your Amazon S3 bucket for image files and to your HTTP server for the HTML files\. 

   1. The origin servers send the files back to the CloudFront edge location\.

   1. As soon as the first byte arrives from the origin, CloudFront begins to forward the files to the user\. CloudFront also adds the files to the cache in the edge location for the next time someone requests those files\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/how-cloudfront-delivers-content.png)

## How CloudFront Works with Regional Edge Caches<a name="CloudFrontRegionaledgecaches"></a>

CloudFront edge locations make sure that popular content can be served quickly to your viewers\. CloudFront also has regional edge caches that bring more of your content closer to your viewers, even when the content is not popular enough to stay at a CloudFront edge location, to help improve performance for that content\.

Regional edge caches help with all types of content, particularly content that tends to become less popular over time\. Examples include user\-generated content, such as video, photos, or artwork; e\-commerce assets such as product photos and videos; and news and event\-related content that might suddenly find new popularity\.

**How Regional Caches Work**  
Regional edge caches are CloudFront locations that are deployed globally, close to your viewers\. They're located between your origin server and the global edge locations that serve content directly to viewers\. As objects become less popular, individual edge locations may remove those objects to make room for more popular content\. Regional edge caches have a larger cache than an individual edge location, so objects remain in the cache longer at the nearest regional edge cache location\. This helps keep more of your content closer to your viewers, reducing the need for CloudFront to go back to your origin server, and improving overall performance for viewers\. 

When a viewer makes a request on your website or through your application, DNS routes the request to the CloudFront edge location that can best serve the user’s request\. This location is typically the nearest CloudFront edge location in terms of latency\. In the edge location, CloudFront checks its cache for the requested files\. If the files are in the cache, CloudFront returns them to the user\. If the files are not in the cache, the edge servers go to the nearest regional edge cache to fetch the object\. 

In the regional edge cache location, CloudFront again checks its cache for the requested files\. If the files are in the cache, CloudFront forwards the files to the requested edge location\. As soon as the first byte arrives from regional edge cache location, CloudFront begins to forward the files to the user\. CloudFront also adds the files to the cache in the requested edge location for the next time someone requests those files\. 

For files not cached at both the edge location and the regional edge cache location, CloudFront compares the request with the specifications in your distributions and forwards the request for your files to the origin server\. After your origin server sends the files back to the regional edge cache location, they are forwarded to the requested edge location, and CloudFront forwards the files to the user\. In this case, CloudFront also adds the files to the cache in the regional edge cache location in addition to the edge location for the next time a viewer requests those files\. This makes sure that all of the edge locations in a region share a local cache, eliminating multiple requests to origin servers\. CloudFront also keeps persistent connections with origin servers so files are fetched from the origins as quickly as possible\.

**Note**  
Regional edge caches have feature parity with edge locations\. For example, a cache invalidation request removes an object from both edge caches and regional edge caches before it expires\. The next time a viewer requests the object, CloudFront returns to the origin to fetch the latest version of the object\.
Proxy methods `PUT/POST/PATCH/OPTIONS/DELETE` go directly to the origin from the edge locations and do not proxy through the Regional Edge Caches\.
Regional edge caches are used for custom origins, but not Amazon S3 origins\.
Dynamic content, as determined at request time \(cache\-behavior configured to forward all headers\), does not flow through regional edge caches, but goes directly to the origin\.