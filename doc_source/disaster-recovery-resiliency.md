# Resilience in Amazon CloudFront<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between Availability Zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

## CloudFront Origin Failover<a name="disaster-recovery-resiliency.origin-failover"></a>

In addition to the support of AWS global infrastructure, Amazon CloudFront offers an *origin failover* feature to help support your data resiliency needs\. CloudFront is a global service that delivers your content through a worldwide network of data centers called *edge locations* or *points of presence* \(POPs\)\. If your content is not already cached in an edge location, CloudFront retrieves it from an origin that you've identified as the source for the definitive version of the content\. 

You can improve resiliency and increase availability for specific scenarios by setting up CloudFront with origin failover\. To get started, you create an origin group in which you designate a primary origin for CloudFront plus a second origin\. CloudFront automatically switches to the second origin when the primary origin returns specific HTTP status code failure responses\. For more information, see [Optimizing High Availability with CloudFront Origin Failover](high_availability_origin_failover.md)\.