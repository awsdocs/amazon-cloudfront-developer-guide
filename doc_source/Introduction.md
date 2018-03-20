# What Is Amazon CloudFront?<a name="Introduction"></a>

Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as \.html, \.css, \.js, and image files, to your users\. CloudFront delivers your content through a worldwide network of data centers called edge locations\. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency \(time delay\), so that content is delivered with the best possible performance\. If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately\. If the content is not in that edge location, CloudFront retrieves it from an Amazon S3 bucket or an HTTP server \(for example, a web server\) that you have identified as the source for the definitive version of your content\. 

This concept is best illustrated by an example\. Suppose you're serving the following image from a traditional web server, not from CloudFront\.

![\[WatchMouse\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/globe_west_540.png)

\(The image is owned by NASA and comes from the Visible Earth website at [http://visibleearth\.nasa\.gov/](http://visibleearth.nasa.gov/)\.\)

You're serving the image using the URL `http://example.com/globe_west_540.png`\. Your users can easily navigate to this URL and see the image, but they probably don't know that their request was routed from one network to another—through the complex collection of interconnected networks that comprise the internet—until the image was found\. 

Further suppose that the web server that you're serving the image from is in Seattle, Washington, USA, and that a user in Austin, Texas, USA requests the image\. The following traceroute list \(courtesy of www\.WatchMouse\.com\) shows one way that this request could be routed\. 

![\[WatchMouse\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/GSG_0001_RouteText.png)

![\[U.S. map\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/GSG_0001_USMapWithRoute.png)

In this example, the request was routed 10 times within the United States before the image was retrieved, which is not an unusually large number of hops \(the network paths between the source and the final destination\)\. If your user were in Europe, the request would be routed through even more networks to reach your server in Seattle\. The number of networks and the distance that the request and the image must travel have a significant impact on the performance, reliability, and availability of the image\. 

CloudFront speeds up the distribution of your content by routing each user request to the edge location that can best serve your content\. Typically, this is the CloudFront edge location that provides the lowest latency\. This dramatically reduces the number of networks that your users' requests must pass through, which improves performance\. Users get lower latency—the time it takes to load the first byte of the file—and higher data transfer rates\. You also get increased reliability and availability because copies of your files \(also known as *objects\)* are now held in multiple edge locations around the world\. 

For a list of the locations of CloudFront edge servers, see the [Amazon CloudFront Product Details page](https://aws.amazon.com/cloudfront/details)\.

**Topics**
+ [How CloudFront Delivers Content](HowCloudFrontWorks.md)
+ [How CloudFront Works with Regional Edge Caches](CloudFrontRegionaledgecaches.md)
+ [Locations and IP Address Ranges of CloudFront Edge Servers](LocationsOfEdgeServers.md)
+ [CloudFront Compliance](compliance.md)