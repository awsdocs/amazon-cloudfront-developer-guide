# CloudFront security savings bundle<a name="savings-bundle"></a>

The CloudFront security savings bundle is a simple way to save up to 30% on the CloudFront charges on your AWS bill when you make an upfront commitment\. When you purchase a savings bundle, you also get credits for [AWS WAF](http://aws.amazon.com/waf/), a web application firewall that helps protect your CloudFront distribution against common web exploits\.

For more information, see the following sections\. To purchase a savings bundle, go the [savings bundle overview page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/savings-bundle/overview)\.

**Topics**
+ [Savings bundle overview](#savings-bundle-overview)
+ [Savings bundle example](#savings-bundle-example)
+ [Purchasing a savings bundle](#savings-bundle-purchasing)
+ [Viewing and updating your savings bundles](#savings-bundle-inventory)
+ [Permissions to manage a savings bundle](#savings-bundle-permissions)
+ [More information about savings bundles](#savings-bundle-details)

## Savings bundle overview<a name="savings-bundle-overview"></a>

Here’s how the CloudFront security savings bundle works:

1. To purchase a savings bundle, you commit to pay a consistent monthly amount \(dollars per month\) for CloudFront for one year\. You are billed for the committed amount each month, for 12 months, starting in the billing period in which you purchase the savings bundle\. If you purchase a savings bundle on the last day of the billing period, the charges and credits begin the following billing period\.

1. In exchange for your commitment, CloudFront automatically applies credits to your AWS bill in each of the 12 billing periods of the one year term\. The value of these credits is more than your committed payment amount, resulting in up to a 30% discount on CloudFront’s standard pricing for the committed amount\. These credits automatically offset CloudFront charges on your AWS bill\. For a detailed example, see the following section\.

1. In addition to the CloudFront credits, you get credits to offset the per\-request charges for using AWS WAF\. The amount of the AWS WAF credits is up to 10% of the amount of the monthly CloudFront commitment\. For more information about using AWS WAF with CloudFront, see [Using AWS WAF to control access to your content](distribution-web-awswaf.md)\.

## Savings bundle example<a name="savings-bundle-example"></a>

Consider a scenario in which your CloudFront usage charges are typically $600 per month\. To take full advantage of the CloudFront security savings bundle, you commit pay $420 for CloudFront each month for one year\. This is 30% less than your typical usage charges \($600 x 0\.7\)\. In exchange for this commitment, CloudFront gives you $600 worth of credits that apply to the CloudFront charges on your monthly AWS bill for each of the next 12 billing periods\. These credits automatically apply to your CloudFront charges, which continue to appear on the bill at standard rates\. In effect, you pay a cost of $420 per month for $600 per month of CloudFront usage\.

In addition, you get $42 in AWS WAF credits to offset the AWS WAF request charges for using AWS WAF with your CloudFront distribution\.

When you purchase a CloudFront security savings bundle at a monthly commitment of $420, your estimated total annual savings is up to $2,664\.

## Purchasing a savings bundle<a name="savings-bundle-purchasing"></a>

To purchase a savings bundle, go to the [savings bundle overview page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/savings-bundle/overview) and choose **Get started**\.

In the first step, the console shows a recommended monthly commitment amount based on your historical usage of CloudFront over the past few months\. You can choose the **Calculator** tab to use the usage calculator to enter your estimated CloudFront usage and receive a recommended monthly commitment amount based on your estimates\.

After you view your recommended commitment and choose **Next**, you can choose the amount of your monthly commitment and whether to automatically renew your savings plan each year\. You can see a summary of the benefits based on your monthly commitment amount\.

![\[Purchase commitment summary of a CloudFront security savings bundle in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/savings-bundle-console.png)

Choose **Next** to review and then purchase your CloudFront security savings bundle\.

## Viewing and updating your savings bundles<a name="savings-bundle-inventory"></a>

To view or update the savings bundles that you purchased, go to the [savings bundle inventory page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/savings-bundle/inventory)\. This page shows the savings bundles that you purchased, and allows you to enable or disable the automatic renewal of a purchased savings bundle\.

You can purchase more than one savings bundle, and you can have multiple savings bundles that are active at the same time\. If you purchase a savings bundle and then find that your monthly CloudFront usage consistently exceeds the credits in the bundle, you can purchase another bundle to get additional savings\.

## Permissions to manage a savings bundle<a name="savings-bundle-permissions"></a>

To manage a CloudFront security savings bundle, the IAM identity must have the required permissions\. Identities with full access to CloudFront \(`cloudfront:*`\) inherit these permissions automatically\. For other identities, you can add the following permissions manually:
+ The following read\-only permissions allow the identity to get information related to existing CloudFront security savings bundles, including the information necessary to view recommendations and estimated savings in the CloudFront console:
  + `cloudfront:ListSavingsPlans`
  + `cloudfront:GetSavingsPlan`
  + `cloudfront:ListRateCards`
  + `cloudfront:ListUsages`
+ `cloudfront:CreateSavingsPlan` – Allows the identity to purchase a CloudFront security savings bundle\.
+ `cloudfront:UpdateSavingsPlan` – Allows the identity to enable or disable the automatic renewal of a purchased CloudFront security savings bundle\.

## More information about savings bundles<a name="savings-bundle-details"></a>

Use the following questions and answers to help you understand additional details about CloudFront security savings bundles\.

**Do the savings bundle credits apply to a specific distribution or all distributions?**  
The credits apply at the AWS account level to all CloudFront usage in the AWS account\.

**Do the credits apply to all types of CloudFront usage?**  
Yes\. The credits apply to all CloudFront charges, including data transfer charges, request charges, and Lambda@Edge charges\.

**Can I use a CloudFront security savings bundle with consolidated billing?**  
Yes, as long as credit sharing is enabled \(you can verify this by viewing the [billing preferences page](https://console.aws.amazon.com/billing/home#/preferences) in the AWS Billing and Cost Management console\)\. You purchase the savings bundle with the payer account \(management account\)\. The credits apply first to any CloudFront charges accrued in the payer account, and then to CloudFront charges accrued in the member accounts, depending on when the account joins or leaves an organization\. For more information about how AWS credits apply across single and multiple accounts, see [AWS credits](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/useconsolidatedbilling-credits.html) in the *AWS Billing and Cost Management User Guide*\.

**What if I don’t use all the credits in a given billing period?**  
Credits are applied to your AWS bill each billing period, and must be used in that billing period\. If any credits are left unused at the end of the billing period, they expire\. Credits do not carry over to the following billing period\.

**What if my CloudFront or AWS WAF usage exceeds the amount of the credits?**  
The CloudFront and AWS WAF charges that you accrue are offset by the credits from the CloudFront security savings bundle\. If your usage exceeds the available credits for that billing period, you are billed for the difference at standard rates\.

**Are the charges or credits prorated for partial months?**  
No\. When you purchase a CloudFront security savings plan, the charge is applied to your bill for the current billing period\. Likewise, the credits are applied to the charges for the current billing period\. If you purchase a savings bundle on the last day of the billing period, the charges and credits begin the following billing period \(the next day\)\.

**What happens when the savings bundle expires?**  
You can choose whether to automatically renew the bundle at the end of the one year term\. If you choose not to automatically renew, the savings bundle expires after the one year term\. When that happens, the credits no longer apply to your AWS bill and you are charged for CloudFront and AWS WAF usage at standard rates\.

**Can I get notifications if my CloudFront usage exceeds the amount covered by the savings bundle credits?**  
Yes\. With AWS Budgets, you can set cost or usage thresholds\. When your actual or forecasted charges for CloudFront exceed a threshold, you get notifications by email or Amazon SNS topic\. You can create a custom budget filtered for CloudFront and set the budget threshold amount to the usage covered by your CloudFront security savings bundle\. For more information, see [Managing your costs with AWS Budgets](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-managing-costs.html) and [Creating a budget](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-create.html) in the *AWS Billing and Cost Management User Guide*\. To get started, go to [AWS Budgets in the console](https://console.aws.amazon.com/billing/home?#/budgets)\.

**How does the CloudFront security savings bundle appear on my bill?**  
The charges for your commitment amount appear in the **CloudFront Security Bundle** section of your monthly bill\. The credits appear in the CloudFront and AWS WAF section of your bill with the description **Usage covered by CloudFront Security Savings Bundle**\.

For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/) on the AWS website\.