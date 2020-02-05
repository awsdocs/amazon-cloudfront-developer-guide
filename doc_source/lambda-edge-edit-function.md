# Editing a Lambda Function for Lambda@Edge<a name="lambda-edge-edit-function"></a>

When you want to edit a Lambda function, note the following:
+ The original version is labeled $LATEST\.
+ You can edit only the $LATEST version\.
+ Each time you edit the $LATEST version, you must publish a new numbered version\.
+ You can't create triggers for $LATEST\.
+ When you publish a new version of a function, Lambda doesn't automatically copy triggers from the previous version to the new version\. You must reproduce the triggers for the new version\. 
+ When you add a trigger for a CloudFront event to a function, if there's already a trigger for the same distribution, cache behavior, and event for an earlier version of the same function, Lambda deletes the trigger from the earlier version\.
+ After you make updates to a CloudFront distribution, like adding triggers, you must wait for the changes to propagate to edge locations before the functions you've specified in the triggers will work\.<a name="lambda-edge-edit-function-procedure"></a>

**To edit a Lambda function \(AWS Lambda console\)**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the Region list at the top of the page, choose **US East \(N\. Virginia\)**\.

1. In the list of functions, choose the name of the function that you want to edit\.

   By default, the console displays the $LATEST version\. You can view earlier versions \(choose **Qualifiers**\), but you can only edit $LATEST\.

1. On the **Code** tab, for **Code entry type**, choose to edit the code in the browser, upload a \.zip file, or upload a file from Amazon S3\.

1. Choose either **Save** or **Save and test**\.

1. Choose **Actions**, and choose **Publish new version**\. 

1. In the **Publish new version from $LATEST** dialog box, enter a description of the new version\. This description appears in the list of versions, along with an automatically generated version number\. 

1. Choose **Publish**\.

   The new version automatically becomes the latest version\. The version number appears on the **Version** button in the upper\-left corner of the page\.

1. Choose the **Triggers** tab\.

1. Choose **Add trigger**\.

1. In the **Add trigger** dialog box, choose the dotted box, and then choose **CloudFront**\.
**Note**  
If you've already created one or more triggers for a function, CloudFront is the default service\.

1. Specify the following values to indicate when you want the Lambda function to execute\.  
**Distribution ID**  
Choose the ID of the distribution that you want to add the trigger to\.  
**Cache behavior**  
Choose the cache behavior that specifies the objects that you want to execute the function on\.  
**CloudFront event**  
Choose the CloudFront event that causes the function to execute\.  
**Enable trigger and replicate**  
Select this check box so Lambda replicates the function to Regions globally\.

1. Choose **Submit**\.

1. To add more triggers for this function, repeat steps 10 through 13\.