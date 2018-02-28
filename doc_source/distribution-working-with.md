# Working with Distributions<a name="distribution-working-with"></a>


+ [Overview of Web and RTMP Distributions](distribution-overview.md)
+ [Creating Web and RTMP Distributions](distribution-create.md)
+ [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md)
+ [Deleting a Distribution](HowToDeleteDistribution.md)
+ [Using Alternate Domain Names \(CNAMEs\)](CNAMEs.md)
+ [Choosing the Price Class for a CloudFront Distribution](PriceClass.md)
+ [Using CloudFront with Amazon S3](MigrateS3ToCloudFront.md)
+ [Changes to the CloudFront API](APIChanges.md)

The following table lists the actions you can perform on a distribution and provides links to the corresponding documentation on how to perform the actions using the CloudFront console and the CloudFront API\.


| Action | Using the CloudFront Console | Using the CloudFront API: Web Distributions | Using the CloudFront API: RTMP Distributions | 
| --- | --- | --- | --- | 
| Create a distribution |  **Web Distributions:** See [Task List for Creating a Web Distribution](distribution-web-creating.md) **RTMP Distributions:** See [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)  | Go to [POST Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateDistribution.html) | Go to [POST Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateStreamingDistribution.html) | 
| List your distributions | See [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md) | Go to [GET Distribution List](http://docs.aws.amazon.com/cloudfront/latest/APIReference/ListDistributions.html)  | Go to [GET Streaming Distribution List](http://docs.aws.amazon.com/cloudfront/latest/APIReference/ListStreamingDistributions.html) | 
| Get all information about a distribution | See [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md) | Go to [GET Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/GetDistribution.html) | Go to [GET Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/GetStreamingDistribution.html) | 
| Get the distribution configuration | See [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md) | Go to [GET Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/GetConfig.html) | Go to [GET Streaming Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/GetStreamingDistConfig.html) | 
| Update a distribution | See [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md) | Go to [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) | Go to [PUT Streaming Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutStreamingDistConfig.html) | 
| Delete a distribution | See [Deleting a Distribution](HowToDeleteDistribution.md) | Go to [DELETE Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/DeleteDistribution.html) | Go to [DELETE Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/DeleteStreamingDistribution.html) | 