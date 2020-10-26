# Using Amazon CloudFront Origin Shield<a name="origin-shield"></a>

CloudFront Origin Shield is an additional layer in the CloudFront caching infrastructure that helps to minimize your origin’s load, improve its availability, and reduce its operating costs\. With CloudFront Origin Shield, you get the following benefits:

**Better cache hit ratio**  
Origin Shield can help improve the cache hit ratio of your CloudFront distribution because it provides an additional layer of caching in front of your origin\. When you use Origin Shield, all requests from all of CloudFront’s caching layers to your origin go through Origin Shield, increasing the likelihood of a cache hit\. CloudFront can retrieve each object with a single origin request from Origin Shield to your origin, and all other layers of the CloudFront cache \(edge locations and [regional edge caches](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches)\) can retrieve the object from Origin Shield\.

**Reduced origin load**  
Origin Shield can further reduce the number of [simultaneous requests](RequestAndResponseBehaviorCustomOrigin.md#request-custom-traffic-spikes) that are sent to your origin for the same object\. Requests for content that is not in Origin Shield’s cache are consolidated with other requests for the same object, resulting in as few as one request going to your origin\. Handling fewer requests at your origin can preserve your origin’s availability during peak loads or unexpected traffic spikes, and can reduce costs for things like just\-in\-time packaging, image transformations, and data transfer out \(DTO\)\.

**Better network performance**  
When you enable Origin Shield in the AWS Region [that has the lowest latency to your origin](#choose-origin-shield-region), you can get better network performance\. For origins in an AWS Region, CloudFront network traffic remains on the high throughput CloudFront network all the way to your origin\. For origins outside of AWS, CloudFront network traffic remains on the CloudFront network all the way to Origin Shield, which has a low latency connection to your origin\.

You incur additional charges for using Origin Shield\. For more information, see [CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

**Topics**
+ [Use cases for Origin Shield](#origin-shield-use-cases)
+ [Choosing the AWS Region for Origin Shield](#choose-origin-shield-region)
+ [Enabling Origin Shield](#enable-origin-shield)
+ [Estimating Origin Shield costs](#origin-shield-costs)
+ [Origin Shield high availability](#origin-shield-high-availability)
+ [How Origin Shield interacts with other CloudFront features](#origin-shield-and-other-features)

## Use cases for Origin Shield<a name="origin-shield-use-cases"></a>

CloudFront Origin Shield can be beneficial for many use cases, including the following:
+ Viewers that are spread across different geographical regions
+ Origins that provide just\-in\-time packaging for live streaming or on\-the\-fly image processing
+ On\-premises origins with capacity or bandwidth constraints
+ Workloads that use multiple content delivery networks \(CDNs\)

Origin Shield may not be a good fit in other cases, such as dynamic content that is proxied to the origin, content with low cacheability, or content that is infrequently requested\.

The following sections explain the benefits of Origin Shield for the following use cases\.

**Topics**
+ [Viewers in different geographical regions](#os-use-cases-global-viewers)
+ [Multiple CDNs](#os-use-cases-multi-cdn)

### Viewers in different geographical regions<a name="os-use-cases-global-viewers"></a>

With Amazon CloudFront, you inherently get a reduced load on your origin because requests that CloudFront can serve from the cache don’t go to your origin\. In addition to CloudFront’s [global network of edge locations](http://aws.amazon.com/cloudfront/features/#Amazon_CloudFront_Infrastructure), [regional edge caches](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches) serve as a mid\-tier caching layer to provide cache hits and consolidate origin requests for viewers in nearby geographical regions\. Viewer requests are routed first to a nearby CloudFront edge location, and if the object isn’t cached in that location, the request is sent on to a regional edge cache\.

When viewers are in different geographical regions, requests can be routed through different regional edge caches, each of which can send a request to your origin for the same content\. But with Origin Shield, you get an additional layer of caching between the regional edge caches and your origin\. All requests from all regional edge caches go through Origin Shield, further reducing the load on your origin\. The following diagrams illustrate this\. In the following diagrams, the origin is AWS Elemental MediaPackage\.

**Without Origin Shield**

Without Origin Shield, your origin might receive duplicate requests for the same content, as shown in the following diagram\.

![\[Without CloudFront Origin Shield, the origin might receive duplicate requests.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origin-shield-without.png)

**With Origin Shield**

Using Origin Shield can help reduce the load on your origin, as shown in the following diagram\.

![\[With CloudFront Origin Shield, the origin can receive fewer duplicate requests.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origin-shield-with.png)

### Multiple CDNs<a name="os-use-cases-multi-cdn"></a>

To serve live video events or popular on\-demand content, you might use multiple content delivery networks \(CDNs\)\. Using multiple CDNs can offer certain advantages, but it also means that your origin might receive many duplicate requests for the same content, each coming from different CDNs or different locations within the same CDN\. These redundant requests might adversely affect the availability of your origin or cause additional operating costs for processes like just\-in\-time packaging or data transfer out \(DTO\) to the internet\.

When you combine Origin Shield with using your CloudFront distribution as the origin for other CDNs, you can get the following benefits:
+ Fewer redundant requests received at your origin, which helps to reduce the negative effects of using multiple CDNs\.
+ A common [cache key](controlling-the-cache-key.md) across CDNs, and centralized management for origin\-facing features\.
+ Improved network performance\. Network traffic from other CDNs is terminated at a nearby CloudFront edge location, which might provide a hit from the local cache\. If the requested object is not in the edge location cache, the request to the origin remains on the CloudFront network all the way to Origin Shield, which provides high throughput and low latency to the origin\. If the requested object is in Origin Shield’s cache, the request to your origin is avoided entirely\.

**Important**  
If you are interested in using Origin Shield in a multi\-CDN architecture, and have discounted pricing, [contact us](http://aws.amazon.com/contact-us/) or your AWS sales representative for more information\. Additional charges may apply\.

The following diagrams show how this configuration can help minimize the load on your origin when you serve popular live video events with multiple CDNs\. In the following diagrams, the origin is AWS Elemental MediaPackage\.

**Without Origin Shield \(multiple CDNs\)**

Without Origin Shield, your origin might receive many duplicate requests for the same content, each coming from a different CDN, as shown in the following diagram\.

![\[Without CloudFront Origin Shield, the origin might receive many duplicate requests, each coming from a different CDN.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origin-shield-without-multi-cdn.png)

**With Origin Shield \(multiple CDNs\)**

Using Origin Shield, with CloudFront as the origin for your other CDNs, can help reduce the load on your origin, as shown in the following diagram\.

![\[With CloudFront Origin Shield, and CloudFront as the origin for other CDNs, the origin receives fewer duplicate requests.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origin-shield-with-multi-cdn.png)

## Choosing the AWS Region for Origin Shield<a name="choose-origin-shield-region"></a>

Amazon CloudFront offers Origin Shield in AWS Regions where CloudFront has a [regional edge cache](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches)\. When you enable Origin Shield, you choose the AWS Region for Origin Shield\. You should choose the AWS Region that has the lowest latency to your origin\. You can use Origin Shield with origins that are in an AWS Region, and with origins that are not in AWS\.

### For origins in an AWS Region<a name="choose-origin-shield-region-inside-aws"></a>

If your origin is in an AWS Region, first determine whether your origin is in a Region in which CloudFront offers Origin Shield\. CloudFront offers Origin Shield in the following AWS Regions\.
+ US East \(Ohio\) – `us-east-2`
+ US East \(N\. Virginia\) – `us-east-1`
+ US West \(Oregon\) – `us-west-2`
+ Asia Pacific \(Mumbai\) – `ap-south-1`
+ Asia Pacific \(Seoul\) – `ap-northeast-2`
+ Asia Pacific \(Singapore\) – `ap-southeast-1`
+ Asia Pacific \(Sydney\) – `ap-southeast-2`
+ Asia Pacific \(Tokyo\) – `ap-northeast-1`
+ Europe \(Frankfurt\) – `eu-central-1`
+ Europe \(Ireland\) – `eu-west-1`
+ Europe \(London\) – `eu-west-2`
+ South America \(São Paulo\) – `sa-east-1`

**If your origin is in an AWS Region in which CloudFront offers Origin Shield**

If your origin is in an AWS Region in which CloudFront offers Origin Shield \(see the preceding list\), enable Origin Shield in the same Region as your origin\.

**If your origin is not in an AWS Region in which CloudFront offers Origin Shield**

 If your origin is not in an AWS Region in which CloudFront offers Origin Shield, see the following table to determine which Region to enable Origin Shield in\.


|  **If your origin is in \.\.\.**  |  **Enable Origin Shield in \.\.\.**  | 
| --- | --- | 
|  US West \(N\. California\) – `us-west-1`  |  US West \(Oregon\) – `us-west-2`  | 
|  Africa \(Cape Town\) – `af-south-1`  |  Europe \(Ireland\) – `eu-west-1`  | 
|  Asia Pacific \(Hong Kong\) – `ap-east-1`  |  Asia Pacific \(Singapore\) – `ap-southeast-1`  | 
|  Canada \(Central\) – `ca-central-1`  |  US East \(N\. Virginia\) – `us-east-1`  | 
|  Europe \(Milan\) – `eu-south-1`  |  Europe \(Frankfurt\) – `eu-central-1`  | 
|  Europe \(Paris\) – `eu-west-3`  |  Europe \(London\) – `eu-west-2`  | 
|  Europe \(Stockholm\) – `eu-north-1`  |  Europe \(London\) – `eu-west-2`  | 
|  Middle East \(Bahrain\) – `me-south-1`  |  Asia Pacific \(Mumbai\) – `ap-south-1`  | 

### For origins outside of AWS<a name="choose-origin-shield-region-outside-aws"></a>

You can use Origin Shield with an origin that is on\-premises or is not in an AWS Region\. In this case, enable Origin Shield in the AWS Region that has the lowest latency to your origin\. If you’re not sure which AWS Region has the lowest latency to your origin, you can use the following suggestions to help you make a determination\.
+ You can consult the preceding table for an approximation of which AWS Region might have the lowest latency to your origin, based on your origin’s geographic location\.
+ You can launch Amazon EC2 instances in a few different AWS Regions that are geographically close to your origin, and run some tests using `ping` to measure the typical network latencies between those Regions and your origin\.

## Enabling Origin Shield<a name="enable-origin-shield"></a>

You can enable Origin Shield to improve your cache hit ratio, reduce the load on your origin, and help improve performance\. To enable Origin Shield, change the origin settings in a CloudFront distribution\. Origin Shield is a property of the origin\. For each origin in your CloudFront distributions, you can separately enable Origin Shield in whichever AWS Region provides the best performance for that origin\.

You can enable Origin Shield in the CloudFront console, with the AWS CloudFormation, or with the CloudFront API\.

### Enabling Origin Shield \(console\)<a name="enable-origin-shield-console"></a>

**To enable Origin Shield for an existing origin \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home)\.

1. Choose the distribution that has the origin that you want to update\.

1. Choose the **Origins and Origin Groups** tab\.

1. Choose the origin to update, then choose **Edit**\.

1. For **Enable Origin Shield**, choose **Yes**\.

1. For **Origin Shield Region**, choose the AWS Region where you want to enable Origin Shield\. For help choosing a Region, see [Choosing the AWS Region for Origin Shield](#choose-origin-shield-region)\.

1. At the bottom of the page, choose **Yes, Edit**\.

When your distribution status is **Deployed**, Origin Shield is ready\. This takes a few minutes\.

**To enable Origin Shield for a new origin \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home)\.

1. To create the new origin in an existing distribution, do the following:

   1. Choose the distribution where you want to create the origin\.

   1. Choose **Create Origin**, and then proceed to step 3\.

   To create the new origin in a new distribution, do the following:

   1. Choose **Create Distribution**\.

   1. In the **Web** section, choose **Get Started**\. In the **Origin Settings** section, complete the following steps, starting with step 3\.

1. For **Enable Origin Shield**, choose **Yes**\.

1. For **Origin Shield Region**, choose the AWS Region where you want to enable Origin Shield\. For help choosing a Region, see [Choosing the AWS Region for Origin Shield](#choose-origin-shield-region)\.

   If you are creating a new distribution, continue configuring your distribution, using the other settings on the page\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. Make sure to save your changes by choosing **Create** \(for a new origin in an existing distribution\) or **Create Distribution** \(for a new origin in a new distribution\)\.

When your distribution status is **Deployed**, Origin Shield is ready\. This takes a few minutes\.

### Enabling Origin Shield \(AWS CloudFormation\)<a name="enable-origin-shield-cloudformation"></a>

To enable Origin Shield with AWS CloudFormation, use the `OriginShield` property in the `Origin` property type in an `AWS::CloudFront::Distribution` resource\. You can add the `OriginShield` property to an existing `Origin`, or include it when you create a new `Origin`\.

The following example shows the syntax, in YAML format, for enabling `OriginShield` in the US West \(Oregon\) Region \(`us-west-2`\)\. For help choosing a Region, see [Choosing the AWS Region for Origin Shield](#choose-origin-shield-region)\. This example shows only the `Origin` property type, not the entire `AWS::CloudFront::Distribution` resource\.

```
Origins:
- DomainName: 3ae97e9482b0d011.mediapackage.us-west-2.amazonaws.com
  Id: Example-EMP-3ae97e9482b0d011
  OriginShield:
    Enabled: true
    OriginShieldRegion: us-west-2
  CustomOriginConfig:
    OriginProtocolPolicy: match-viewer
    OriginSSLProtocols: TLSv1
```

For more information, see [AWS::CloudFront::Distribution Origin](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-cloudfront-distribution-origin.html) in the resource and property reference section of the *AWS CloudFormation User Guide*\.

### Enabling Origin Shield \(CloudFront API, AWS SDKs, AWS CLI\)<a name="enable-origin-shield-api-cli"></a>

To enable Origin Shield with the CloudFront API using the AWS SDKs or AWS Command Line Interface \(AWS CLI\), use the `OriginShield` type\. You specify `OriginShield` in an `Origin`, in a `DistributionConfig`\. For information about the `OriginShield` type, see the following information in the *Amazon CloudFront API Reference*\.
+ [OriginShield](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_OriginShield.html) \(type\)
+ [Origin](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_Origin.html) \(type\)
+ [DistributionConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DistributionConfig.html) \(type\)
+ [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) \(operation\)
+ [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) \(operation\)

The specific syntax for using these types and operations varies based on the SDK, CLI, or API client\. For more information, see the reference documentation for your SDK, CLI, or client\.

## Estimating Origin Shield costs<a name="origin-shield-costs"></a>

You accrue charges for Origin Shield based on the number of requests that go to Origin Shield as an incremental layer\.

For dynamic \(non\-cacheable\) requests that are proxied to the origin, Origin Shield is always an incremental layer\. Dynamic requests use the following HTTP methods: `PUT`, `POST`, `PATCH`, and `DELETE`\.

To estimate your charges for Origin Shield for dynamic requests, use the following formula:

Total number of dynamic requests **x** Origin Shield charge per 10,000 requests **/** 10,000

For cacheable requests \(HTTP methods `GET`, `HEAD`, and `OPTIONS`\), Origin Shield is sometimes an incremental layer\. When you enable Origin Shield, you choose the AWS Region for Origin Shield\. For requests that naturally go to the [regional edge cache](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches) in the same region as Origin Shield, Origin Shield is not an incremental layer\. You do not accrue Origin Shield charges for these requests\. For requests that go to a regional edge cache in a different region from Origin Shield, and then go to Origin Shield, Origin Shield is an incremental layer\. You do accrue Origin Shield charges for these requests\.

To estimate your charges for Origin Shield for cacheable requests, use the following formula:

Total number of cacheable requests **x** \(1 – cache hit rate\) **x** percentage of requests that go to Origin Shield from a regional edge cache in a different region **x** Origin Shield charge per 10,000 requests **/** 10,000

For more information about the charge per 10,000 requests for Origin Shield, see [CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

## Origin Shield high availability<a name="origin-shield-high-availability"></a>

Origin Shield leverages Amazon CloudFront’s [regional edge caches](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches)\. Each of these edge caches is built in an AWS Region using at least three [Availability Zones](http://aws.amazon.com/about-aws/global-infrastructure/regions_az/) with fleets of auto\-scaling Amazon EC2 instances\. Connections from CloudFront locations to Origin Shield also use active error tracking for each request to automatically route the request to a secondary Origin Shield location if the primary Origin Shield location is unavailable\.

## How Origin Shield interacts with other CloudFront features<a name="origin-shield-and-other-features"></a>

The following sections explain how Origin Shield interacts with other CloudFront features\.

### Origin Shield and CloudFront logging<a name="origin-shield-logging"></a>

To see when Origin Shield handled a request, you must enable one of the following:
+ [CloudFront standard logs \(access logs\)](AccessLogs.md)\. Standard logs are provided free of charge\.
+ [CloudFront real\-time logs](real-time-logs.md)\. You incur additional charges for using real\-time logs\. See [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

Cache hits from Origin Shield appear as `OriginShieldHit` in the `x-edge-detailed-result-type` field in CloudFront logs\. Origin Shield leverages Amazon CloudFront’s [regional edge caches](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches)\. If a request is routed from a CloudFront edge location to the regional edge cache that is acting as Origin Shield, it is reported as a `Hit` in the logs, not as an `OriginShieldHit`\.

### Origin Shield and origin groups<a name="origin-shield-and-origin-group"></a>

Origin Shield is compatible with [CloudFront origin groups](high_availability_origin_failover.md)\. Because Origin Shield is a property of the origin, requests always travel through Origin Shield for each origin even when the origin is part of an origin group\. For a given request, CloudFront routes the request to the primary origin in the origin group through the primary origin’s Origin Shield\. If that request fails \(according to the origin group failover criteria\), CloudFront routes the request to the secondary origin through the secondary origin’s Origin Shield\.

### Origin Shield and Lambda@Edge<a name="origin-shield-and-lambda-at-edge"></a>

Origin Shield does not impact the functionality of [Lambda@Edge](lambda-at-the-edge.md) functions, but it can affect the AWS Region where those functions run\. When you use Origin Shield with Lambda@Edge, [origin\-facing triggers](lambda-cloudfront-trigger-events.md) \(origin request and origin response\) run in the AWS Region where Origin Shield is enabled\. Viewer\-facing triggers are not affected\.