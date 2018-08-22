# Setting Up Amazon CloudFront<a name="setting-up-cloudfront"></a>

The overview and procedures in this section help you get started with AWS\.

**Topics**
+ [Sign Up for AWS](#setting-up-sign-up-for-aws)
+ [Access Your Account](#setting-up-access-account)
+ [Create an IAM User](#setting-up-create-iam-user)
+ [Set Up the AWS Command Line Interface or AWS Tools for Windows PowerShell](#setting-up-aws-cli)
+ [Download an AWS SDK](#setting-up-sdk)

## Sign Up for AWS<a name="setting-up-sign-up-for-aws"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services in AWS, including Amazon CloudFront\. You are charged only for the services that you use\.

If you have an AWS account already, skip to [Access Your Account](#setting-up-access-account)\. Otherwise, go ahead and create one\.<a name="setting-up-sign-up-for-aws-procedure"></a>

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

Note your AWS account number, because you'll need it later\.

**Tip**  
If you plan to use CloudFront to distribute content that you store in an S3 bucket, make sure that you also complete the steps to sign up for S3\. For more information, see [Sign Up for Amazon S3](http://docs.aws.amazon.com/AmazonS3/latest/gsg/SigningUpforS3.html)\.

## Access Your Account<a name="setting-up-access-account"></a>

You use AWS services by using any of the following options:
+ AWS Management Console
+ API for each service
+ AWS Command Line Interface \(AWS CLI\)
+ AWS Tools for Windows PowerShell
+ AWS SDKs

For each of those options, you need to access your AWS account by providing credentials that verify that you have permissions to use the services\.

### Access the Console<a name="setting-up-access-account-console"></a>

To access the AWS Management Console for the first time, you provide an email address and a password\. This combination of your email address and password is called your *root identity* or *root account credentials*\. After you access your account for the first time, we strongly recommend that you don't use your root account credentials again for everyday use\. Instead, you should create new credentials by using [AWS Identity and Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. To do that, you create a user account for yourself known as an *IAM user*, and then add the IAM user to an IAM group with administrative permissions or grant the IAM user administrative permissions\. You then can access AWS using a special URL and the credentials for the IAM user\. You also can add other IAM users later, and restrict their access to specified resources in the account\.

**Note**  
Some ad\-blocking plugins for web browsers interfere with Amazon CloudFront console operations, which can cause the console to behave unpredictably\. If you installed an ad\-blocking plugin for your browser, we recommend that you add the URL for the CloudFront console, [https://console\.aws\.amazon\.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home), to the whitelist for the plugin\.

### Access the API, AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDKs<a name="setting-up-access-account-api-cli"></a>

To use the API, the AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDKs, you must create *access keys*\. These keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\.

To create the keys, you sign in to the AWS Management Console\. We strongly recommend that you sign in with your IAM user credentials instead of your root credentials\. For more information, see [Managing Access Keys for IAM Users](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

## Create an IAM User<a name="setting-up-create-iam-user"></a>

Use the following procedures to create a group for administrators, create an IAM user, and then add the IAM user to the administrators group\. If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\. If you aren't familiar with using the console, see [Working with the AWS Management Console](http://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html) for an overview\. 

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](http://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** type **Administrators**\.

1. For **Filter policies**, select the check box for **AWS managed \- job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.<a name="setting-up-sign-in-iam-user-procedure"></a>

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

For more information about using IAM, see [AWS Authentication and Access Control for CloudFront](auth-and-access-control.md)\.

## Set Up the AWS Command Line Interface or AWS Tools for Windows PowerShell<a name="setting-up-aws-cli"></a>

The AWS Command Line Interface \(AWS CLI\) is a unified tool for managing AWS services\. For information about how to install and configure the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

If you have experience with Windows PowerShell, you might prefer to use AWS Tools for Windows PowerShell\. For more information, see [Setting up the AWS Tools for Windows PowerShell](http://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide*\.

## Download an AWS SDK<a name="setting-up-sdk"></a>

If you're using a programming language that AWS provides an SDK for, we recommend that you use an SDK instead of the Amazon CloudFront API\. The SDKs make authentication simpler, integrate easily with your development environment, and provide easy access to CloudFront commands\. For more information, see [Tools for Amazon Web Services](https://aws.amazon.com//tools/)\.