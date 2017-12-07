# How CloudFront Delivers Content<a name="HowCloudFrontWorks"></a>

After some initial setup, CloudFront works invisibly to speed up delivery of your content\. This overview includes both the steps you perform before your first user accesses your application or website and how CloudFront serves your content when configuration is complete\. 

Setting up CloudFront involves a few simple steps:

**How You Configure CloudFront to Deliver Your Content**

1. You configure your *origin servers*, from which CloudFront gets your files for distribution from CloudFront edge locations all over the world\. 

   An origin server stores the original, definitive version of your objects\. If you're serving content over HTTP, your origin server is either an Amazon S3 bucket or an HTTP server, such as a web server\. Your HTTP server can run on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance or on a server that you manage; these servers are also known as *custom origins\.*

   If you distribute media files on demand using the Adobe Media Server RTMP protocol, your origin server is always an Amazon S3 bucket\.

1. You upload your files to your origin servers\. Your files, also known as *objects*, typically include web pages, images, and media files, but can be anything that can be served over HTTP or a supported version of Adobe RTMP, the protocol used by Adobe Flash Media Server\.

   If you're using an Amazon S3 bucket as an origin server, you can make the objects in your bucket publicly readable, so that anyone who knows the CloudFront URLs for your objects can access them\. You also have the option of keeping objects private and controlling who accesses them\. See [Serving Private Content through CloudFront](PrivateContent.md)\. 

1. You create a CloudFront *distribution*, which tells CloudFront which origin servers to get your files from when users request the files through your web site or application\. At the same time, you specify details such as whether you want CloudFront to log all requests and whether you want the distribution to be enabled as soon as it's created\.

1. CloudFront assigns a domain name to your new distribution and displays it in the CloudFront console or returns it in the response to a programmatic request, for example, an API request\.

1. CloudFront sends your distribution's configuration \(but not your content\) to all of its **edge locations**—collections of servers in geographically dispersed data centers where CloudFront caches copies of your objects\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

As you develop your website or application, you use the domain name that CloudFront provides for your URLs\. For example, if CloudFront returns `d111111abcdef8.cloudfront.net` as the domain name for your distribution, the URL for logo\.jpg in your Amazon S3 bucket \(or in the root directory on an HTTP server\) will be `http://d111111abcdef8.cloudfront.net/logo.jpg`\.

You can also configure your CloudFront distribution so you can use your own domain name\. In that case, the URL might be `http://www.example.com/logo.jpg`\.

Optionally, you can configure your origin server to add headers to the files; the headers indicate how long you want the files to stay in the cache in CloudFront edge locations\. By default, each object stays in an edge location for 24 hours before it expires\. The minimum expiration time is 0 seconds; there isn't a maximum expiration time limit\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

**How CloudFront Delivers Content to Your Users**

Once you configure CloudFront to deliver your content, here's what happens when users request your objects:

1. A user accesses your website or application and requests one or more objects, such as an image file and an HTML file\.

1. DNS routes the request to the CloudFront edge location that can best serve the user's request, typically the nearest CloudFront edge location in terms of latency, and routes the request to that edge location\. 

1. In the edge location, CloudFront checks its cache for the requested files\. If the files are in the cache, CloudFront returns them to the user\. If the files are *not* in the cache, it does the following:

   1. CloudFront compares the request with the specifications in your distribution and forwards the request for the files to the applicable origin server for the corresponding file type—for example, to your Amazon S3 bucket for image files and to your HTTP server for the HTML files\. 

   1. The origin servers send the files back to the CloudFront edge location\.

   1. As soon as the first byte arrives from the origin, CloudFront begins to forward the files to the user\. CloudFront also adds the files to the cache in the edge location for the next time someone requests those files\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)