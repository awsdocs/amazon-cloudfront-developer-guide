# Choosing the price class for a CloudFront distribution<a name="PriceClass"></a>

CloudFront has [edge locations all over the world](http://aws.amazon.com/cloudfront/features/#Global_Edge_Network)\. Our cost for each edge location varies and, as a result, the price that we charge varies depending on which edge location serves the requests\.

CloudFront edge locations are grouped into geographic regions, and we’ve grouped regions into price classes as shows in the following table\. You choose a price class when you [create](distribution-web-creating-console.md) or [update](HowToUpdateDistribution.md) a CloudFront distribution\.


|  | North America \(United States, Mexico, Canada\) | Europe and Palestine | South Africa, Kenya, and the Middle East | South America | Japan | Australia and New Zealand | Hong Kong, Indonesia, the Philippines, Singapore, South Korea, Taiwan, and Thailand | India | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Price Class All | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes | 
| Price Class 200 | Yes | Yes | Yes | No | Yes | No | Yes | Yes | 
| Price Class 100 | Yes | Yes | No | No | No | No | No | No | 

By default, CloudFront responds to requests based only on performance\. Objects are served from the edge location that has the lowest latency for the viewer\. If you’re willing to accept potentially higher latency for viewers in some geographic regions in return for lower cost, you can choose a price class that doesn’t include all geographic regions\. Some viewers, especially those in geographic regions that are not in your price class, might see higher latency than if your content was served from all CloudFront edge locations\. For example, if you choose **Price Class 100**, viewers in India might experience higher latency than if you choose **Price Class 200**\.

If you choose a price class that doesn’t include all edge locations, CloudFront might still occasionally serve requests from an edge location in a region that is not included in your price class\. When this happens, you are not charged the rate for the more expensive region\. Instead, you’re charged the rate for the least expensive region in your price class\.

For more information about CloudFront pricing and price classes, see [ Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing)\.
