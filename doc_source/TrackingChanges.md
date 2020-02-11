# Tracking Configuration Changes with AWS Config<a name="TrackingChanges"></a>

You can use AWS Config to record configuration changes for CloudFront distribution settings changes\. For example, you can capture changes to distribution states, price classes, origins, geo restriction settings, and Lambda@Edge configurations\. 

**Note**  
AWS Config does not record key–value tags for CloudFront distributions\.

## Set up AWS Config with CloudFront<a name="TrackingChangesSettings"></a>

When you set up AWS Config, you can choose to record all supported AWS resources, or you can specify only certain resources to record configuration changes for, such as just recording changes for CloudFront\. To see the specific resources supported for CloudFront, see the list of [ Supported AWS Resource Types](https://docs.aws.amazon.com/config/latest/developerguide/resource-config-reference.html#supported-resources) in the *AWS Config Developer Guide*\. 

To track configuration changes to your CloudFront distribution, you must log in to the AWS Console in the US East \(N\. Virginia\) public region\.

**Note**  
There might be a delay in recording resources with AWS Config\. AWS Config records resources only after it discovers the resources\.<a name="HowToSetUpAWSConfigProcedure"></a>

**Set up AWS Config with CloudFront by using the AWS Management Console**

1. Sign in to the AWS Management Console and open the AWS Config console at [https://console\.aws\.amazon\.com/config/](https://console.aws.amazon.com/config/)\.

1. Choose **Get Started Now**\.

1. On the **Settings** page, for **Resource types to record**, specify the AWS resource types that you want AWS Config to record\. If you want to record only CloudFront changes, choose **Specific types**, and then, under **CloudFront**, choose the distribution or streaming distribution that you want to track changes for\.

   To add or change which distributions to track, choose **Settings** on the left, after completing your initial setup\.

1. Specify additional required options for AWS Config: set up a notification, specify a location for the configuration information, and add rules for evaluating resource types\.

For more information, see [Setting up AWS Config with the Console](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html) in the *AWS Config Developer Guide*\.

To set up AWS Config with CloudFront by using the AWS CLI or by using an API, see one of the following:
+ **Use the AWS CLI:** [Setting up AWS Config with the AWS CLI](https://docs.aws.amazon.com/config/latest/developerguide/gs-cli.html) in the *AWS Config Developer Guide*
+ **Use an API:** The [ StartConfigurationRecorder](https://docs.aws.amazon.com/config/latest/APIReference/API_StartConfigurationRecorder.html) action and other information in the *AWS Config API Reference*

## View CloudFront Configuration History<a name="TrackingChangesGetHistory"></a>

After AWS Config starts recording configuration changes to your distributions, you can get the configuration history of any distribution that you have configured for CloudFront\.

You can view configuration histories in any of the following ways:
+ **Use the AWS Config console\.** For each recorded resource, you can view a timeline page, which provides a history of configuration details\. To view this page, choose the gray icon in the **Config Timeline** column of the **Dedicated Hosts** page\. For more information, see [Viewing Configuration Details in the AWS Config Console](https://docs.aws.amazon.com/config/latest/developerguide/view-manage-resource-console.html) in the *AWS Config Developer Guide*\.
+ **Run AWS CLI commands\.** To get a list of all your distributions, use the [ list\-discovered\-resources](http://docs.aws.amazon.com/cli/latest/reference/configservice/list-discovered-resources.html) command\. To get the configuration details of a distribution for a specific time interval, use the [ get\-resource\-config\-history](http://docs.aws.amazon.com/cli/latest/reference/configservice/get-resource-config-history.html) command\. For more information, see [View Configuration Details Using the CLI](https://docs.aws.amazon.com/config/latest/developerguide/resource-config-reference.html) in the *AWS Config Developer Guide*\.
+ **Use the AWS Config API in your applications\. ** To get a list of all your distributions use the [ListDiscoveredResources](https://docs.aws.amazon.com/config/latest/APIReference/API_ListDiscoveredResources.html) action\. To get the configuration details of a distribution for a specific time interval, use the [GetResourceConfigHistory](https://docs.aws.amazon.com/config/latest/APIReference/API_GetResourceConfigHistory.html) action\. For more information, see the [AWS Config API Reference](https://docs.aws.amazon.com/config/latest/APIReference/)\.

For example, to get a list of all of your distributions from AWS Config, you could run a CLI command such as the following:

`aws configservice list-discovered-resources --resource-type AWS::CloudFront::Distribution`

Or, to get a list of all of your RTMP streaming distributions from AWS Config, run a CLI command such as the following:

`aws configservice list-discovered-resources --resource-type AWS::CloudFront::StreamingDistribution`