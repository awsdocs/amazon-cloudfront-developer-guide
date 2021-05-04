# Associating functions with distributions<a name="associate-function"></a>

To use a function in CloudFront Functions with a CloudFront distribution, you associate the function with one or more cache behaviors in the distribution\. You can associate a function with multiple cache behaviors in multiple distributions\. Before you associate a function, you must [publish it](publish-function.md) to the `LIVE` stage\.

When you associate a function with a cache behavior, you must choose an *event type*\. The event type determines when CloudFront Functions runs the function\. There are two event types to choose from:

For more information about event types, see [CloudFront Events That Can Trigger a Lambda Function](lambda-cloudfront-trigger-events.md)\. You cannot use origin\-facing event types \(*origin request* and *origin response*\) with CloudFront Functions\.
+ **Viewer request** – The function runs when CloudFront receives a request from a viewer\.
+ **Viewer response** – The function runs before CloudFront returns a response to the viewer\.

You can associate a function with a distribution in the CloudFront console or with the AWS CLI\.

## Associating functions with distributions \(console\)<a name="associate-function-console"></a>

You can use the CloudFront console to associate a function with an existing cache behavior in an existing CloudFront distribution\. For more information about creating a distribution, see [Creating a Distribution](distribution-web-creating-console.md)\.

**To associate a function with an existing cache behavior \(console\)**

1. To associate a function with a distribution, open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home#/functions](https://console.aws.amazon.com/cloudfront/v2/home#/functions), and then choose the function that you want to associate\.

1. On the function page, choose the **Associate** tab\. Then do the following:

   1. For **Distribution**, choose a distribution to associate the function with\.

   1. For **Event type**, choose when you want this function to run:
      + To run the function every time CloudFront receives a request, choose **Viewer Request**\.
      + To run the function every time CloudFront returns a response, choose **Viewer Response**\.

   1. For **Cache behavior**, choose a cache behavior to associate this function with \(choose **\*** for the default cache behavior\)\. The function runs when the request \(or in the case of a viewer response function, the response’s corresponding request\) matches this cache behavior\.

   1. Choose **Add association**\. Then, in the **Associate function to cache behavior** pop\-up window, choose **Associate**\.  
![\[Associate tab of a function page in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-associate.png)

When successful, you see a banner at the top of the page that says ***Function name* associated successfully**\. You also see the associated distribution in the **Associated CloudFront distributions** table\. Wait a few minutes for the associated distribution to finish deploying\. To check the distribution’s status, choose the associated distribution, then choose **View distribution**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-view-distribution.png)

The distribution’s `Status` changes to `InProgress` while the distribution is redeployed\. As soon as the new distribution configuration reaches a CloudFront edge location, that edge location begins using the associated function\. When the distribution is fully deployed, the `Status` changes back to `Deployed`, which indicates that the associated CloudFront function is live in all CloudFront edge locations worldwide\. This typically takes a few minutes\.

## Associating functions with distributions \(AWS CLI\)<a name="associate-function-cli"></a>

You can associate a function with an existing cache behavior, with a new cache behavior in an existing distribution, or with a new cache behavior in a new distribution\. The following procedure shows how to associate a function with an existing cache behavior\. You can associate a function with a new cache behavior \(in an existing or new distribution\) using a process similar to what’s described here\.

**To associate a function with an existing cache behavior \(AWS CLI\)**
**Note**  
The following example commands use input files and output that are formatted as a YAML string\. This feature is only available in version 2 of the AWS CLI\. For more information, see [Setting the AWS CLI output format]() in the *AWS Command Line Interface User Guide*\.  
If you’re using version 1 of the AWS CLI, you can follow a similar process using JSON for the input files and output\.

1. Use the following command to save the distribution configuration for the distribution whose cache behavior you want to associate with a function\. This command saves the distribution configuration to a file named `dist-config.yaml`\. To use this command, do the following:
   + Replace *`DistributionID`* with the distribution’s ID\.
   + Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

   ```
   aws cloudfront get-distribution-config \
       --id DistributionID \
       --output yaml > dist-config.yaml
   ```

   When the command is successful, the AWS CLI returns no output\.

1. Open the file named `dist-config.yaml` that you just created\. Edit the file, making the following changes:

   1. Rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

   1. In the cache behavior, find the object named `FunctionAssociations`\. Update this object to add a function association\. The YAML syntax for a function association looks like the following example\.
      + The following example shows a viewer request event type \(trigger\)\. To use a viewer response event type, replace `viewer-request` with `viewer-response`\.
      + Replace *`arn:aws:cloudfront::111122223333:function/ExampleFunction`* with the Amazon Resource Name \(ARN\) of the function that you’re associating with this cache behavior\. To get the function ARN, you can use the aws cloudfront list\-functions command\.

      ```
      FunctionAssociations:
        Items:
          - EventType: viewer-request
            FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
        Quantity: 1
      ```

   After making these changes, save the file\.

1. Use the following command to update the distribution, adding the function association\. To use this command, do the following:
   + Replace *`DistributionID`* with the distribution’s ID\.
   + Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

   ```
   aws cloudfront update-distribution \
       --id DistributionID \
       --cli-input-yaml file://dist-config.yaml
   ```

   When the command is successful, you see output like the following that describes the distribution that was just updated with the function association\. The following example output is truncated for readability\.

   ```
   Distribution:
     ARN: arn:aws:cloudfront::111122223333:distribution/EBEDLT3BGRBBW
     ... truncated ...
     DistributionConfig:
       ... truncated ...
       DefaultCacheBehavior:
         ... truncated ...
         FunctionAssociations:
           Items:
           - EventType: viewer-request
             FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
           Quantity: 1
         ... truncated ...
     DomainName: d111111abcdef8.cloudfront.net
     Id: EDFDVBD6EXAMPLE
     LastModifiedTime: '2021-04-19T22:39:09.158000+00:00'
     Status: InProgress
   ETag: E2VJGGQEG1JT8S
   ```

   When you update a distribution, the distribution’s `Status` changes to `InProgress` while the distribution is redeployed\. As soon as the new distribution configuration reaches a CloudFront edge location, that edge location begins using the associated function\. When the distribution is fully deployed, the `Status` changes back to `Deployed`, which indicates that the associated CloudFront function is live in all CloudFront edge locations worldwide\. This typically takes a few minutes\.