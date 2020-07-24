# Setting up Amazon CloudFront<a name="setting-up-cloudfront"></a>

The overview and procedures in this section help you get started with AWS\.

**Topics**
+ [Sign up for AWS](#setting-up-sign-up-for-aws)
+ [Access your account](#setting-up-access-account)
+ [Create an IAM user](#setting-up-create-iam-user)
+ [Set up the AWS Command Line Interface or AWS Tools for Windows PowerShell](#setting-up-aws-cli)
+ [Download an AWS SDK](#setting-up-sdk)

## Sign up for AWS<a name="setting-up-sign-up-for-aws"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including Amazon CloudFront\. You are charged only for the services that you use\.

If you have an AWS account already, skip to [Access your account](#setting-up-access-account)\. Otherwise, create one\.<a name="setting-up-sign-up-for-aws-procedure"></a>

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

Note your AWS account number, because you'll need it later\.

**Tip**  
If you plan to use CloudFront to distribute content that you store in an S3 bucket, make sure that you also complete the steps to sign up for S3\. For more information, see [Sign Up for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/gsg/SigningUpforS3.html)\.

## Access your account<a name="setting-up-access-account"></a>

You use AWS services by using any of the following options:
+ AWS Management Console
+ API for each service
+ AWS Command Line Interface \(AWS CLI\)
+ AWS Tools for Windows PowerShell
+ AWS SDKs

For each of those options, you need to access your AWS account by providing credentials that verify that you have permissions to use the services\.

### Access the console<a name="setting-up-access-account-console"></a>

To access the AWS Management Console for the first time, you provide an email address and a password\. This combination of your email address and password is called your *root identity* or *root account credentials*\. After you access your account for the first time, we strongly recommend that you don't use your root account credentials again for everyday use\. Instead, you should create new credentials by using [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. To do that, you create a user account for yourself known as an *IAM user*, and then add the IAM user to an IAM group with administrative permissions or grant the IAM user administrative permissions\. You then can access AWS using a special URL and the credentials for the IAM user\. You also can add other IAM users later, and restrict their access to specified resources in the account\.

**Note**  
Some ad\-blocking plugins for web browsers interfere with Amazon CloudFront console operations, which can cause the console to behave unpredictably\. If you installed an ad\-blocking plugin for your browser, we recommend that you add the URL for the CloudFront console, [https://console\.aws\.amazon\.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home), to the whitelist for the plugin\.

### Access the API, AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDKs<a name="setting-up-access-account-api-cli"></a>

To use the API, the AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDKs, you must create *access keys*\. These keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\.

To create the keys, you sign in to the AWS Management Console\. We strongly recommend that you sign in with your IAM user credentials instead of your root credentials\. For more information, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

## Create an IAM user<a name="setting-up-create-iam-user"></a>

Use the following procedures to create a group for administrators, create an IAM user, and then add the IAM user to the administrators group\. If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\. If you aren't familiar with using the console, see [Working with the AWS Management Console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html) for an overview\. 

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \-job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.<a name="setting-up-sign-in-iam-user-procedure"></a>

**To sign in as your new IAM user**

1. Sign out of the AWS console\.

1. Sign in by using the following URL, where *your\_aws\_account\_id* is your AWS account number without the hyphens\. For example, if your AWS account number is 1234\-5678\-9012, your AWS account ID is 123456789012:

   ```
   https://your_aws_account_id.signin.aws.amazon.com/console/
   ```

1. Enter the IAM user name \(not your email address\) and password that you just created\. When you're signed in, the navigation bar displays "*your\_user\_name* @ *your\_aws\_account\_id*"\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\.<a name="setting-up-create-account-alias-procedure"></a>

**To create an account alias and conceal your account ID**

1. On the IAM console, choose **Dashboard** in the navigation pane\. 

1. On the dashboard, choose **Customize** and enter an alias such as your company name\.

1. Sign out of the AWS console\.

1. Sign in by using the following URL:

   ```
   https://your_account_alias.signin.aws.amazon.com/console/
   ```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **IAM users sign\-in link** on the dashboard\.

For more information about using IAM, see [Identity and Access Management in CloudFront](auth-and-access-control.md)\.

## Set up the AWS Command Line Interface or AWS Tools for Windows PowerShell<a name="setting-up-aws-cli"></a>

The AWS Command Line Interface \(AWS CLI\) is a unified tool for managing AWS services\. For information about how to install and configure the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

If you have experience with Windows PowerShell, you might prefer to use AWS Tools for Windows PowerShell\. For more information, see [Setting up the AWS Tools for Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide*\.

## Download an AWS SDK<a name="setting-up-sdk"></a>

If you're using a programming language that AWS provides an SDK for, we recommend that you use an SDK instead of the Amazon CloudFront API\. The SDKs make authentication simpler, integrate easily with your development environment, and provide easy access to CloudFront commands\. For more information, see [Tools to Build on AWS](http://aws.amazon.com/tools/)\.