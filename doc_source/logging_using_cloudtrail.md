# Using AWS CloudTrail to Capture Requests Sent to the CloudFront API<a name="logging_using_cloudtrail"></a>

CloudFront is integrated with CloudTrail, an AWS service that captures information about every request that is sent to the CloudFront API by your AWS account, including your IAM users\. CloudTrail periodically saves log files of these requests to an Amazon S3 bucket that you specify\. CloudTrail captures information about all requests, whether they were made using the CloudFront console, the CloudFront API, the AWS SDKs, the CloudFront CLI, or another service, for example, AWS CloudFormation\.

You can use information in the CloudTrail log files to determine which requests were made to CloudFront, the source IP address from which each request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the *[AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)*\.

**Note**  
CloudFront is a global service\. To view CloudFront requests in CloudTrail logs, you must update an existing trail to include global services\. For more information, see [Updating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) and [About Global Service Events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html#cloudtrail-concepts-global-service-events) in the *AWS CloudTrail User Guide*\. 

**Topics**
+ [CloudFront Information in CloudTrail](#service-name-info-in-cloudtrail)
+ [Understanding CloudFront Log File Entries](#understanding_cloudfront_entries)

## CloudFront Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in CloudFront, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. Because CloudFront is a global service, events for the service are logged in US East \(N\. Virginia\)\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for CloudFront, create a trail\. Your trail must include global service events\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions and includes global service events\. The trail logs events from all regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All CloudFront API actions are logged by CloudTrail and are documented in the [Amazon CloudFront API Reference](https://docs.aws.amazon.com/cloudfront/latest/APIReference/)\. For example, calls to the `CreateDistribution`, `GetDistribution` and `ListInvalidations ` APIs generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Understanding CloudFront Log File Entries<a name="understanding_cloudfront_entries"></a>

Each JSON\-formatted CloudTrail log file can contain one or more log entries\. A log entry represents a single request from any source and includes information about the requested action, including any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order; they are not an ordered stack trace of API calls\.

The `eventName` element identifies the action that occurred and the API version that was used to perform that action\. For example, the following `eventName` value indicates that a web distribution was updated, and the 2014\-01\-31 API version was used to perform the action:

`UpdateDistribution2014_01_31`

The following example shows a CloudTrail log entry that demonstrates five actions:
+ Updating a web distribution configuration\. The value of `eventName` is `UpdateDistribution`\.
+ Listing web distributions that are associated with the current account\. The value of `eventName` is `ListDistributions`\.
+ Getting the configuration for a specific web distribution\. The value of `eventName` is `GetDistribution`\.
+ Creating an invalidation batch request\. The value of `eventName` is `CreateInvalidation`\.
+ Listing origin access identities that are associated with the current account\. The value of `eventName` is `ListCloudFrontOriginAccessIdentities`\.

```
{
   "Records": [{
      "eventVersion": "1.01",
      "userIdentity": {
         "type": "IAMUser",
         "principalId": "A1B2C3D4E5F6G7EXAMPLE",
         "arn": "arn:aws:iam::111122223333:user/smithj",
         "accountId": "111122223333",
         "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
         "userName": "smithj"
      },
      "eventTime": "2014-05-06T18:00:32Z",
      "eventName": "UpdateDistribution2014_01_31",
      "sourceIPAddress": "192.0.2.17",
      "userAgent": "aws-sdk-ruby/1.39.0 ruby/1.9.3 x86_64-linux",
      "requestParameters": {
         "id": "EDFDVBD6EXAMPLE",
         "ifMatch": "E9LHASXEXAMPLE",
         "distributionConfig": {
            "restrictions": {
               "geoRestriction": {
                  "quantity": 0,
                  "restrictionType": "none"
               }
            },
            "customErrorResponses": {
               "quantity": 0
            },
            "defaultRootObject": "index.html",
            "aliases": {
               "quantity": 1,
               "items": ["example.com"]
            },
            "logging": {
               "bucket": "",
               "enabled": false,
               "prefix": "",
               "includeCookies": false
            },
            "viewerCertificate": {
               "iAMCertificateId": "A1B2C3D4E5F6G7EXAMPLE",
               "sSLSupportMethod": "sni-only"
            },
            "callerReference": "2014-05-06 64832",
            "defaultCacheBehavior": {
               "targetOriginId": "Images",
               "allowedMethods": {
                  "items": ["GET",
                  "HEAD"],
                  "quantity": 2
               },
               "forwardedValues": {
                  "cookies": {
                     "forward": "none"
                  },
                  "queryString": false
               },
               "minTTL": 300,
               "trustedSigners": {
                  "enabled": false,
                  "quantity": 0
               },
               "viewerProtocolPolicy": "redirect-to-https",
               "smoothStreaming": false
            },
            "origins": {
               "items": [{
                  "customOriginConfig": {
                     "hTTPSPort": 443,
                     "originProtocolPolicy": "http-only",
                     "hTTPPort": 80
                  },
                  "domainName": "myawsbucket.s3-website-us-east-2.amazonaws.com",
                  "id": "Web page origin"
               },
               {
                  "customOriginConfig": {
                     "hTTPSPort": 443,
                     "originProtocolPolicy": "http-only",
                     "hTTPPort": 80
                  },
                  "domainName": "myotherawsbucket.s3-website-us-west-2.amazonaws.com",
                  "id": "Images"
               }],
               "quantity": 2
            },
            "enabled": true,
            "cacheBehaviors": {
                  "allowedMethods": {
                     "items": ["GET",
                     "HEAD"],
                     "quantity": 2
                  },
                  "trustedSigners": {
                     "enabled": false,
                     "quantity": 0
                  },
                  "targetOriginId": "Web page origin",
                  "smoothStreaming": false,
                  "viewerProtocolPolicy": "redirect-to-https",
                  "minTTL": 300,
                  "forwardedValues": {
                     "cookies": {
                        "forward": "none"
                     },
                     "queryString": false
                  },
                  "pathPattern": "*.html"
               }],
               "quantity": 1
            },
            "priceClass": "PriceClass_All",
            "comment": "Added an origin and a cache behavior"
         }
      },
      "responseElements": {
         "eTag": "E2QWRUHEXAMPLE",
         "distribution": {
            "domainName": "d111111abcdef8.cloudfront.net",
            "status": "InProgress",
            "distributionConfig": {
            distributionConfig response omitted
            },
            "id": "EDFDVBD6EXAMPLE",
            "lastModifiedTime": "May 6, 2014 6:00:32 PM",
            "activeTrustedSigners": {
               "quantity": 0,
               "enabled": false
            },
            "inProgressInvalidationBatches": 0
         }
      },
      "requestID": "4e6b66f9-d548-11e3-a8a9-73e33example",
      "eventID": "5ab02562-0fc5-43d0-b7b6-90293example"
   },
   {
      "eventVersion": "1.01",
      "userIdentity": {
         "type": "IAMUser",
         "principalId": "A1B2C3D4E5F6G7EXAMPLE",
         "arn": "arn:aws:iam::111122223333:user/smithj",
         "accountId": "111122223333",
         "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
         "userName": "smithj"
      },
      "eventTime": "2014-05-06T18:01:35Z",
      "eventName": "ListDistributions2014_01_31",
      "sourceIPAddress": "192.0.2.17",
      "userAgent": "aws-sdk-ruby/1.39.0 ruby/1.9.3 x86_64-linux",
      "requestParameters": null,
      "responseElements": null,
      "requestID": "52de9f97-d548-11e3-8fb9-4dad0example",
      "eventID": "eb91f423-6dd3-4bb0-a148-3cdfbexample"
   },
   {
      "eventVersion": "1.01",
      "userIdentity": {
         "type": "IAMUser",
         "principalId": "A1B2C3D4E5F6G7EXAMPLE",
         "arn": "arn:aws:iam::111122223333:user/smithj",
         "accountId": "111122223333",
         "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
         "userName": "smithj"
      },
      "eventTime": "2014-05-06T18:01:59Z",
      "eventName": "GetDistribution2014_01_31",
      "sourceIPAddress": "192.0.2.17",
      "userAgent": "aws-sdk-ruby/1.39.0 ruby/1.9.3 x86_64-linux",
      "requestParameters": {
         "id": "EDFDVBD6EXAMPLE"
      },
      "responseElements": null,
      "requestID": "497b3622-d548-11e3-8fb9-4dad0example",
      "eventID": "c32289c7-005a-46f7-9801-cba41example"
   },
   {
      "eventVersion": "1.01",
      "userIdentity": {
         "type": "IAMUser",
         "principalId": "A1B2C3D4E5F6G7EXAMPLE",
         "arn": "arn:aws:iam::111122223333:user/smithj",
         "accountId": "111122223333",
         "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
         "userName": "smithj"
      },
      "eventTime": "2014-05-06T18:02:27Z",
      "eventName": "CreateInvalidation2014_01_31",
      "sourceIPAddress": "192.0.2.17",
      "userAgent": "aws-sdk-ruby/1.39.0 ruby/1.9.3 x86_64-linux",
      "requestParameters": {
         "invalidationBatch": {
            "callerReference": "2014-05-06 64947",
            "paths": {
               "quantity": 3,
               "items": ["/images/new.jpg",
               "/images/logo.jpg",
               "/images/banner.jpg"]
            }
         },
         "distributionId": "EDFDVBD6EXAMPLE"
      },
      "responseElements": {
         "invalidation": {
            "createTime": "May 6, 2014 6:02:27 PM",
            "invalidationBatch": {
               "callerReference": "2014-05-06 64947",
               "paths": {
                  "quantity": 3,
                  "items": ["/images/banner.jpg",
                  "/images/logo.jpg",
                  "/images/new.jpg"]
               }
            },
            "status": "InProgress",
            "id": "ISRZ85EXAMPLE"
         },
         "location": "https://cloudfront.amazonaws.com/2014-01-31/distribution/EDFDVBD6EXAMPLE/invalidation/ISRZ85EXAMPLE"
      },
      "requestID": "4e200613-d548-11e3-a8a9-73e33example",
      "eventID": "191ebb93-66b7-4517-a741-92b0eexample"
   },
   {
      "eventVersion": "1.01",
      "userIdentity": {
         "type": "IAMUser",
         "principalId": "A1B2C3D4E5F6G7EXAMPLE",
         "arn": "arn:aws:iam::111122223333:user/smithj",
         "accountId": "111122223333",
         "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
         "userName": "smithj"
      },
      "eventTime": "2014-05-06T18:03:08Z",
      "eventName": "ListCloudFrontOriginAccessIdentities2014_01_31",
      "sourceIPAddress": "192.0.2.17",
      "userAgent": "aws-sdk-ruby/1.39.0 ruby/1.9.3 x86_64-linux",
      "requestParameters": null,
      "responseElements": null,
      "requestID": "42ca4299-d548-11e3-8fb9-4dad0example",
      "eventID": "7aeb434f-eb55-4e2a-82d8-417d5example"
   }]
}
```