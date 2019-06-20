# Monitoring CloudFront and Setting Alarms<a name="monitoring-using-cloudwatch"></a>

Amazon CloudFront is integrated with CloudWatch, and automatically publishes 10 operational metrics per distribution, displayed in a set of graphs in the CloudFront console in the AWS Management Console or accessible by using the CloudFront API or CLI\. These metrics don't count against [CloudWatch limits](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_limits.html)\.

In the console, you can monitor your CloudFront distributions and the Lambda functions associated with them, as well as troubleshoot issues, by viewing metrics and using other functionality, such as access to regional log files, to help you track and debug issues\.

The following metrics are included for no additional cost:
+ Requests
+ Bytes downloaded
+ Bytes uploaded
+ 4xx error rate
+ 5xx error rate
+ 5xx error rate for Lambda@Edge
+ Total error rate
+ Lambda execution errors
+ Lambda invalid responses
+ Lambda throttles

The **Monitoring** page in the console includes the following:
+ A list of all of your CloudFront distributions and a list of all of the Lambda@Edge functions that are associated with your distributions\. You can choose a distribution or function to select, and then view metrics associated with it\.
+ A view of distribution metrics that includes aggregated Lambda@Edge function HTTP 5xx errors, grouped by distribution, to help you see whether CloudFront HTTP 5xx errors are caused by your origins or by a Lambda@Edge function\.
+ A group of Lambda@Edge metrics graphs that show a regional breakdown of metrics by function execution errors, invalid function responses errors, and throttles\.

To view graphs about the activity for a specific CloudFront distribution or Lambda function, choose one, and then choose to view the metrics\.

You can also set alarms based on these metrics in the CloudFront console, or you can set alarms in the CloudWatch console with standard CloudWatch rates\. For example, you can set an alarm based on 5xxErrorRate metric in the CloudWatch console\. This metric represents the percentage of all requests for which the HTTP status code is 5xx\.

**Topics**
+ [CloudFront Distribution Metrics](#monitoring-console.distributions)
+ [Lambda@Edge Function Metrics](#monitoring-console.lambda-at-edge)
+ [Setting Alarms to Receive Notifications](#receiving-notifications)

## CloudFront Distribution Metrics<a name="monitoring-console.distributions"></a>

Metrics for each CloudFront distribution are included on graphs on the **Overview** tab\. On each graph, the totals are displayed at 1\-minute granularity, and you can specify a time range and refresh rate\. You can also choose to download reports as CSV files \(see [Downloading Data in CSV Format](cloudwatch-csv.md)\)\.

![\[Graphs for CloudFront distribution on the Monitoring page in the console\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

### Viewing Metrics for a Distribution<a name="viewing-metrics"></a>

To view detailed graphs about a distribution, such as error rate metrics, follow these steps\.<a name="monitoring-using-cloudwatch-procedure"></a>

**To view metrics for a distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, click **Monitoring**, then choose a distribution\.

1. Choose **View distribution metrics**\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh interval\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

#### Viewing Metrics for a Distribution<a name="viewing-metrics-tabs"></a>

Metrics for distributions are shown on two tabs:

*On the **Overview** tab*, graphs include metrics about total requests, data transfer, and the HTTP error rate, shown as a percentage of total requests\.

*On the **Lambda@Edge errors** tab*, the console shows graphs for Lambda function execution errors, invalid function responses, and throttling by AWS Region for a distribution\. On the graphs are the number of errors returned by your Lambda@Edge functions for each type, grouped by AWS Region\. This gives you a starting point for troubleshooting specifically where an error comes from so that you can address it\.

If you see a spike in errors that you want to investigate, for example, you can choose a function and then view log files by Region, until you determine which function is causing the problems and in which Region\. For more information about troubleshooting Lambda@Edge errors, see [How to Determine the Type of Failure](lambda-edge-testing-debugging.md#lambda-edge-testing-debugging-failure-type)\.

For an example about how to use this information in troubleshooting HTTP errors, see [Four steps for debugging your content delivery on AWS](https://aws-blogs-prod.amazon.com/networking-and-content-delivery/four-steps-for-debugging-your-content-delivery-on-aws/)\.

## Lambda@Edge Function Metrics<a name="monitoring-console.lambda-at-edge"></a>

The **Monitoring** page in the AWS Management Console displays a list of all of the Lambda@Edge functions that are associated with the distributions in your account\. You choose a specific function, and then view several graphs about that function's activity\. For each graph, you can specify a time range and refresh rate\.

![\[Choose a Lambda@Edge function on the Monitoring page in the console\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

The graphs for each Lambda function include the number of invocations, errors, throttles, and so on\. On each graph, the totals are displayed at 1\-minute granularity, grouped by AWS Region\.

![\[Graphs about a specific Lambda@Edge function associated with a distribution\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

### Viewing Metrics for a Function<a name="viewing-function-metrics"></a>

To view detailed graphs about a function, such as invocations by Region, follow these steps\.<a name="monitoring-viewing-function-metrics-procedure"></a>

**To view metrics for a function**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, choose **Monitoring**\.

1. Under **Lambda@Edge functions**, choose a function, and then choose **View function metrics**\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh interval\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view the graphs in the CloudWatch console, choose **Add to dashboard**\.

## Setting Alarms to Receive Notifications<a name="receiving-notifications"></a>

In the CloudFront console, you can set alarms to notify you by Amazon Simple Notification Service based on specific CloudFront metrics\.<a name="alarming-using-cloudwatch-procedure"></a>

**To receive an Amazon Simple Notification Service \(Amazon SNS\) notification based on a CloudFront metric**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, click **Alarms**, then choose a distribution\.

1. On the **Alarms** page, expand the list of existing alarms to confirm that the alarm that you want to create doesn't already exist\.

1. Click **Create Alarm**\.

1. In the **Create Alarm** dialog box, specify the following values:   
**Metric**  
Choose the metric for which you want to create the alarm\.  
**Distribution**  
Choose the CloudFront distribution for which you want to create the alarm\.  
****Name of alarm****  
Enter a name for the alarm\.  
**Send notification to**  
Choose the existing Amazon SNS topic that you want to send notification to if the status of this metric triggers an alarm\.   
****Whenever** metric *operator value***  
Specify when CloudWatch should trigger an alarm and send a notification to the specified email list\. For example, to receive notification when the 5xx error rate exceeds 1%, you'd specify the following:  
**Whenever Average of 5xxErrorRate** `> 1`  
Note the following about specifying values for *value*:  
   + Enter only whole numbers without punctuation\. For example, to specify one thousand, enter **1000**\.
   + For 4xx, 5xx, and total error rates, the value that you specify is a percentage\.
   + For requests, bytes downloaded, and bytes uploaded, the value you specify is in units, for example, 1000000000 bytes\.  
****For at least** *x* **consecutive periods of** *time period***  
Specify how many consecutive time periods of the specified duration the metric must meet the criteria before CloudWatch sends notification\. When you choose a value, you need to find an appropriate balance between a value that produces frequent notifications for fleeting problems and delayed notifications for real problems\.

1. If you created a new Amazon SNS topic, when you click **Create**, Amazon SNS sends you an email with information about the new topic\. Follow the instructions in the email\. 

## Amazon CloudFront Metrics<a name="cloudfront-metrics"></a>

The `AWS/CloudFront` namespace includes the following metrics\.

**Note**  
Only one statistic, Average or Sum, is applicable for each metric\. However, all statistics are available through the console, API, and AWS Command Line Interface\. In the following table, each metric specifies the statistic that is applicable to that metric\.


| Metric | Description | 
| --- | --- | 
|  `Requests` |  The number of requests for all HTTP methods and for both HTTP and HTTPS requests\. Valid Statistics: Sum Units: None  | 
|  `BytesDownloaded` |  The number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\. Valid Statistics: Sum Units: None  | 
| `BytesUploaded` | The number of bytes uploaded to your origin with CloudFront using `POST` and `PUT` requests\. Valid Statistics: Sum Units: None  | 
| `TotalErrorRate` | The percentage of all requests for which the HTTP status code is `4xx` or `5xx`\. Valid Statistics: Average Units: Percent  | 
| `4xxErrorRate` | The percentage of all requests for which the HTTP status code is `4xx`\. Valid Statistics: Average Units: Percent  | 
| `5xxErrorRate` | The percentage of all requests for which the HTTP status code is `5xx`\. Valid Statistics: Average Units: Percent  | 

## Dimensions for CloudFront Metrics<a name="cloudfront-metricdimensions"></a>

CloudFront metrics use the CloudFront namespace and provide metrics for two dimensions:


| Dimension | Description | 
| --- | --- | 
| `DistributionId` | The CloudFront ID of the distribution for which you want to display metrics\. | 
| `Region` | The region for which you want to display metrics\. This value must be `Global`\. The `Region` dimension is different from the region in which CloudFront metrics are stored, which is US East \(N\. Virginia\)\.  | 