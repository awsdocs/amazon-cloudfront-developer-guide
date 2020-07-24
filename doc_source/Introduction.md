# What is Amazon CloudFront?<a name="Introduction"></a>

Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as \.html, \.css, \.js, and image files, to your users\. CloudFront delivers your content through a worldwide network of data centers called edge locations\. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency \(time delay\), so that content is delivered with the best possible performance\.
+ If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately\.
+ If the content is not in that edge location, CloudFront retrieves it from an origin that you've defined—such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server \(for example, a web server\) that you have identified as the source for the definitive version of your content\.

As an example, suppose that you're serving an image from a traditional web server, not from CloudFront\. For example, you might serve an image, sunsetphoto\.png, using the URL `http://example.com/sunsetphoto.png`\.

Your users can easily navigate to this URL and see the image\. But they probably don't know that their request was routed from one network to another—through the complex collection of interconnected networks that comprise the internet—until the image was found\.

CloudFront speeds up the distribution of your content by routing each user request through the AWS backbone network to the edge location that can best serve your content\. Typically, this is a CloudFront edge server that provides the fastest delivery to the viewer\. Using the AWS network dramatically reduces the number of networks that your users' requests must pass through, which improves performance\. Users get lower latency—the time it takes to load the first byte of the file—and higher data transfer rates\.

You also get increased reliability and availability because copies of your files \(also known as *objects\)* are now held \(or cached\) in multiple edge locations around the world\. 

**Topics**
+ [How you set up CloudFront to deliver content](#HowCloudFrontWorksOverview)
+ [CloudFront use cases](IntroductionUseCases.md)
+ [How CloudFront delivers content](HowCloudFrontWorks.md)
+ [Locations and IP address ranges of CloudFront edge servers](LocationsOfEdgeServers.md)
+ [Accessing CloudFront](introduction-accessing-cloudfront.md)
+ [How to get started with Amazon CloudFront](welcome-how-to-get-started.md)
+ [AWS Identity and Access Management](IAMCloudFront.md)
+ [CloudFront pricing](CloudFrontPricing.md)

## How you set up CloudFront to deliver content<a name="HowCloudFrontWorksOverview"></a>

You create a CloudFront distribution to tell CloudFront where you want content to be delivered from, and the details about how to track and manage content delivery\. Then CloudFront uses computers—edge servers—that are close to your viewers to deliver that content quickly when someone wants to see it or use it\.

![\[How CloudFront works\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/how-you-configure-cf.png)<a name="HowCloudFrontWorksConfiguration"></a>

**How you configure CloudFront to deliver your content**

1. You specify *origin servers*, like an Amazon S3 bucket or your own HTTP server, from which CloudFront gets your files which will then be distributed from CloudFront edge locations all over the world\. 

   An origin server stores the original, definitive version of your objects\. If you're serving content over HTTP, your origin server is either an Amazon S3 bucket or an HTTP server, such as a web server\. Your HTTP server can run on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance or on a server that you manage; these servers are also known as *custom origins\.*

   If you use the Adobe Flash Media Server RTMP protocol to distribute media files on demand, your origin server is always an Amazon S3 bucket\.

1. You upload your files to your origin servers\. Your files, also known as *objects*, typically include web pages, images, and media files, but can be anything that can be served over HTTP or a supported version of Adobe RTMP, the protocol used by Adobe Flash Media Server\.

   If you're using an Amazon S3 bucket as an origin server, you can make the objects in your bucket publicly readable, so that anyone who knows the CloudFront URLs for your objects can access them\. You also have the option of keeping objects private and controlling who accesses them\. See [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\. 

1. You create a CloudFront *distribution*, which tells CloudFront which origin servers to get your files from when users request the files through your web site or application\. At the same time, you specify details such as whether you want CloudFront to log all requests and whether you want the distribution to be enabled as soon as it's created\.

1. CloudFront assigns a domain name to your new distribution that you can see in the CloudFront console, or that is returned in the response to a programmatic request, for example, an API request\. If you like, you can add an alternate domain name to use instead\.

1. CloudFront sends your distribution's configuration \(but not your content\) to all of its *edge locations* or *points of presence* \(POPs\)— collections of servers in geographically\-dispersed data centers where CloudFront caches copies of your files\.

As you develop your website or application, you use the domain name that CloudFront provides for your URLs\. For example, if CloudFront returns `d111111abcdef8.cloudfront.net` as the domain name for your distribution, the URL for logo\.jpg in your Amazon S3 bucket \(or in the root directory on an HTTP server\) is `http://d111111abcdef8.cloudfront.net/logo.jpg`\.

Or you can set up CloudFront to use your own domain name with your distribution\. In that case, the URL might be `http://www.example.com/logo.jpg`\.

Optionally, you can configure your origin server to add headers to the files, to indicate how long you want the files to stay in the cache in CloudFront edge locations\. By default, each file stays in an edge location for 24 hours before it expires\. The minimum expiration time is 0 seconds; there isn't a maximum expiration time\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.