# Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)<a name="private-content-trusted-signers"></a>

**Topics**
+ [Creating CloudFront Key Pairs for Your Trusted Signers](#private-content-creating-cloudfront-key-pairs)
+ [Reformatting the CloudFront Private Key \(\.NET and Java Only\)](#private-content-reformatting-private-key)
+ [Adding Trusted Signers to Your Distribution](#private-content-adding-trusted-signers)
+ [Verifying that Trusted Signers Are Active \(Optional\)](#private-content-verifying-trusted-signers-active)
+ [Rotating CloudFront Key Pairs](#private-content-rotating-key-pairs)

To create signed URLs or signed cookies, you need at least one AWS account that has an active CloudFront key pair\. This account is known as a trusted signer\. The trusted signer has two purposes:
+ As soon as you add the AWS account ID for your trusted signer to your distribution, CloudFront starts to require that users use signed URLs or signed cookies to access your files\.
+ When you create signed URLs or signed cookies, you use the private key from the trusted signer's key pair to sign a portion of the URL or the cookie\. When someone requests a restricted file, CloudFront compares the signed portion of the URL or cookie with the unsigned portion to verify that the URL or cookie hasn't been tampered with\. CloudFront also verifies that the URL or cookie is valid, meaning, for example, that the expiration date and time hasn't passed\.

When you specify trusted signers, you also indirectly specify the files that require signed URLs or signed cookies:
+ **Web distributions** – You add trusted signers to cache behaviors\. If your distribution has only one cache behavior, users must use signed URLs or signed cookies to access any file associated with the distribution\. If you create multiple cache behaviors and add trusted signers to some cache behaviors and not to others, you can require that users use signed URLs or signed cookies to access some files and not others\.
+ **RTMP distributions \(signed URLs only\)** – You add trusted signers to a distribution\. After you add trusted signers to an RTMP distribution, users must use signed URLs to access any file associated with the distribution\.

**Note**  
To specify trusted signers for a distribution, you must use the CloudFront console or CloudFront API version 2009\-09\-09 or later\.

To specify the accounts that are allowed to create signed URLs or signed cookies and to add the accounts to your CloudFront distribution, do the following tasks:

1. Decide which AWS accounts you want to use as trusted signers\. Most CloudFront customers use the account that they used to create the distribution\.

1. For each of the accounts that you selected in Step 1, create a CloudFront key pair\. For more information, see [Creating CloudFront Key Pairs for Your Trusted Signers](#private-content-creating-cloudfront-key-pairs)\.

1. If you're using \.NET or Java to create signed URLs or signed cookies, reformat the CloudFront private key\. For more information, see [Reformatting the CloudFront Private Key \(\.NET and Java Only\)](#private-content-reformatting-private-key)\.

1. In the distribution for which you're creating signed URLs or signed cookies, specify the AWS account IDs of your trusted signers\. For more information, see [Adding Trusted Signers to Your Distribution](#private-content-adding-trusted-signers)\.

1. \(Optional\) Verify that CloudFront recognizes that your trusted signers have active CloudFront key pairs\. For more information, see [Verifying that Trusted Signers Are Active \(Optional\)](#private-content-verifying-trusted-signers-active)\.

## Creating CloudFront Key Pairs for Your Trusted Signers<a name="private-content-creating-cloudfront-key-pairs"></a>

Each of the AWS accounts that you use to create CloudFront signed URLs or signed cookies—your trusted signers—must have its own CloudFront key pair, and the key pair must be active\. Note that you can't substitute an Amazon EC2 key pair for a CloudFront key pair\. When you create a CloudFront signed URL or signed cookie, you include the key pair ID for the trusted signer's key pair in the URL\. Amazon EC2 does not make key pair IDs available\.

To help secure your applications, we recommend that you change CloudFront key pairs every 90 days or more often\. For more information, see [Rotating CloudFront Key Pairs](#private-content-rotating-key-pairs)\.

You can create a key pair in the following ways:
+ Create a key pair in the AWS Management Console and download the private key\. See the procedure [To create CloudFront key pairs in the AWS Management Console](#private-content-creating-cloudfront-key-pairs-procedure)\.
+ Create an RSA key pair by using an application such as OpenSSL, and upload the public key to the AWS Management Console\. See the procedure [To create an RSA key pair and upload the public key in the AWS Management Console](#private-content-uploading-cloudfront-public-key-procedure)\.<a name="private-content-creating-cloudfront-key-pairs-procedure"></a>

**To create CloudFront key pairs in the AWS Management Console**

1. Sign in to the AWS Management Console using the root credentials for an AWS account\.
**Important**  
IAM users can't create CloudFront key pairs\. You must log in using root credentials to create key pairs\.

1. On the *account\-name* menu, click **Security Credentials**\.

1. Expand **CloudFront Key Pairs**\.

1. Confirm that you have no more than one active key pair\. You can't create a key pair if you already have two active key pairs\.

1. Click **Create New Key Pair**\.

1. In the **Create Key Pair** dialog box, click **Download Private Key File**\.

1. In the **Opening <filename>** dialog box, accept the default value of **Save File**, and click **OK** to download and save the private key for your CloudFront key pair\.
**Important**  
Save the private key for your CloudFront key pair in a secure location, and set permissions on the file so that only the desired administrator users can read it\. If someone gets your private key, they can generate valid signed URLs and signed cookies and download your content\. You cannot get the private key again, so if you lose or delete it, you must create a new CloudFront key pair\. 

1. Record the key pair ID for your key pair\. \(In the AWS Management Console, this is called the access key ID\.\) You'll use it when you create signed URLs or signed cookies\.<a name="private-content-uploading-cloudfront-public-key-procedure"></a>

**To create an RSA key pair and upload the public key in the AWS Management Console**

1. Use OpenSSL or another tool to create a key pair\. 

   For example, if you're using OpenSSL, you can use the following command to generate a key pair with a length of 4096 bits and save it in the file `private_key.pem`:

   ```
   openssl genrsa -out private_key.pem 4096
   ```

   The resulting file contains both the public and the private key\. To extract the public key from that file, run the following command:

   ```
   openssl rsa -pubout -in private_key.pem -out public_key.pem
   ```

   The public key \(`public_key.pem`\) is the file that you upload later in this procedure\.

   Note the following requirements for the key:
   + The key pair must be an SSH\-2 RSA key pair\.
   + The key pair must be in base64 encoded PEM format\.
   + The supported key lengths are 1024, 2048, and 4096 bits\.

1. Sign in to the AWS Management Console using the root credentials for an AWS account\.
**Important**  
IAM users can't create CloudFront key pairs\. You must log in using root credentials to create key pairs\.

1. On the *account\-name* menu, click **Security Credentials**\.

1. Expand **CloudFront Key Pairs**\.

1. Confirm that you have no more than one active key pair\. You can't upload your own key pair if you already have two active key pairs\.

1. Click **Upload Your Own Key Pair**\.

1. In the **Upload Your Own Key Pair** dialog box, click **Choose File** and choose the public key file that you created in step 1\.

1. Click **Upload**\.

   The **Upload Key Pair** dialog box clears, and the new key pair appears at the top of the list of CloudFront key pairs\.

1. Record the key pair ID for your key pair\. \(In the AWS Management Console, this is called the access key ID\.\) You'll use it when you create signed URLs or signed cookies\.

## Reformatting the CloudFront Private Key \(\.NET and Java Only\)<a name="private-content-reformatting-private-key"></a>

If you're using \.NET or Java to create signed URLs or signed cookies, you cannot use the private key from your key pair in the default \.pem format to create the signature:
+ **\.NET framework** – Convert the private key to the XML format that the \.NET framework uses\. Several tools are available\.   
+ **Java** – Convert the private key to DER format\. To do this, you can use OpenSSL:

  `$ openssl pkcs8 -topk8 -nocrypt -in origin.pem -inform PEM -out new.der -outform DER`

  To ensure that the encoder works correctly, add the jar for the Bouncy Castle Java cryptography APIs to your project and then add the Bouncy Castle provider\.

## Adding Trusted Signers to Your Distribution<a name="private-content-adding-trusted-signers"></a>

Trusted signers are the AWS accounts that can create signed URLs and signed cookies for a distribution\. By default, no account, not even the account that created the distribution, is allowed to create signed URLs or signed cookies\. To specify the AWS accounts that you want to use as trusted signers, add the accounts to your distribution:
+ **Web distributions** – Trusted signers are associated with cache behaviors\. This allows you to require signed URLs or signed cookies for some files and not for others in the same distribution\. Trusted signers can only create signed URLs or cookies for files that are associated with the corresponding cache behaviors\. For example, if you have one trusted signer for one cache behavior and a different trusted signer for a different cache behavior, neither trusted signer can create signed URLs or cookies for files that are associated with the other cache behavior\.
+ **RTMP distributions \(signed URLs only\)** – Trusted signers are associated with the distribution\. After you add trusted signers to an RTMP distribution, users must use signed URLs or signed cookies to access any of the objects associated with the distribution\.

**Important**  
Define path patterns and their sequence carefully so you don't either give users unintended access to your content or prevent them from accessing content that you want to be available to everyone\. For example, suppose a request matches the path pattern for two cache behaviors\. The first cache behavior does not require signed URLs or signed cookies and the second cache behavior does\. Users will be able to access the files without using signed URLs or signed cookies because CloudFront processes the cache behavior that is associated with the first match\.

For more information about path patterns, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.

**Important**  
If you're updating a distribution that you're already using to distribute content, add trusted signers only when you're ready to start generating signed URLs or signed cookies for your files, or CloudFront will reject the requests:  
**Web distributions** – After you add trusted signers to a cache behavior for a web distribution, users must use signed URLs or signed cookies to access the files that are associated with the cache behavior\.
**RTMP distributions \(signed URLs only\)** – After you add trusted signers to an RTMP distribution, users must use signed URLs to access any of the files associated with the distribution\. 

The maximum number of trusted signers depends on the type of distribution:
+ **Web distributions** – A maximum of five for each cache behavior
+ **RTMP distributions** – A maximum of five for the distribution

You can add trusted signers to your distribution using either the CloudFront console or the CloudFront API\. See the following topic:
+ [Adding Trusted Signers to Your Distribution Using the CloudFront Console](#private-content-adding-trusted-signers-console)
+ [Adding Trusted Signers to Your Distribution Using the CloudFront API](#private-content-adding-trusted-signers-api)

### Adding Trusted Signers to Your Distribution Using the CloudFront Console<a name="private-content-adding-trusted-signers-console"></a><a name="private-content-adding-trusted-signers-console-procedure"></a>

**To add trusted signers to your distribution using the CloudFront console**

1. If you want to use only the AWS account that created the distribution as a trusted signer, skip to Step 2\.

   If you want to use other AWS accounts, get the AWS account ID for each account:

   1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/console/home](https://console.aws.amazon.com/console/home) using an account that you want to use as a trusted signer\.

   1. In the upper\-right corner of the console, click the name associated with the account, and click **My Account**\.

   1. Under **Account Settings**, make note of the account ID\.

   1. Sign out of the AWS Management Console\.

   1. Repeat steps a through d for the other accounts that you want to use as trusted signers\.

1. Open the Amazon CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/), and sign in using the account that you used to create the distribution that you want to add trusted signers to\.

1. Click the distribution ID\.

1. Change to edit mode:
   + **Web distributions** – Click the **Behaviors** tab, click the behavior that you want to edit, and click **Edit**\.
   + **RTMP distributions** – Click **Edit**\.

1. For **Restrict Viewer Access \(Use Signed URLs or Signed Cookies\)**, click **Yes**\.

1. For **Trusted Signers**, select the check boxes for your scenario:
   + **Self** – Select this check box if you want to use the current account \(the account that you used to create the distribution\)\.
   + **Specify Accounts** – Select this check box if you want to use other AWS accounts\.

1. If you selected the **Specify Accounts** check box, enter AWS account IDs in the **AWS Account Number** field\. These are the account IDs that you got in the first step of this procedure\. Enter one account ID per line\.

1. Click **Yes, Edit**\.

1. If you're adding trusted signers to a web distribution and you have more than one cache behavior, repeat steps 4 through 8\.

### Adding Trusted Signers to Your Distribution Using the CloudFront API<a name="private-content-adding-trusted-signers-api"></a>

You can use the CloudFront API to add the AWS account IDs for trusted signers to an existing distribution or to create a new distribution that includes trusted signers\. In either case, specify the values in the `TrustedSigners` element\. For web distributions, add the `TrustedSigners` element to one or more cache behaviors\. For RTMP distributions, add the `TrustedSigners` element to the distribution\.

See the following topics in the *Amazon CloudFront API Reference*:
+ **Create a new web distribution** – [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)
+ **Update an existing web distribution** – [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)
+ **Create a new RTMP distribution** – [CreateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateStreamingDistribution.html)
+ **Update an existing RTMP distribution** – [UpdateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateStreamingDistribution.html)

## Verifying that Trusted Signers Are Active \(Optional\)<a name="private-content-verifying-trusted-signers-active"></a>

After you add trusted signers to your distribution, you might want to verify that the signers are active\. For a trusted signer to be active, the following must be true:
+ The AWS account must have at least one active key pair\. If you're rotating key pairs, the account will temporarily have two active key pairs, the old key pair and the new one\.
+ CloudFront must be aware of the active key pair\. After you create a key pair, there can be a short period of time before CloudFront is aware that the key pair exists\.

**Note**  
To display a list of active trusted signers for a distribution, you currently must use the CloudFront API\. A list of active trusted signers is not available in the CloudFront console\.

### Verifying that Trusted Signers Are Active Using the CloudFront API<a name="private-content-verifying-trusted-signers-active-api"></a>

To determine which trusted signers have active key pairs \(are active trusted signers\), you get the distribution and review the values in the `ActiveTrustedSigners` element\. This element lists the AWS account ID of each account that the distribution identifies as a trusted signer\. If the trusted signer has one or more active CloudFront key pairs, the `ActiveTrustedSigners` element also lists the key pair IDs\. For more information, see the following topics in the *Amazon CloudFront API Reference*:
+ **Web distributions** – [ GetDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetDistribution.html)
+ **RTMP distributions** – [ GetStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetStreamingDistribution.html)

## Rotating CloudFront Key Pairs<a name="private-content-rotating-key-pairs"></a>

AWS recommends that you rotate \(change\) your active CloudFront key pairs every 90 days\. To rotate CloudFront key pairs that you're using to create signed URLs or signed cookies without invalidating URLs or cookies that haven't expired yet, do the following tasks:

1. Create a new key pair for each of the accounts that you're using to create signed URLs\. For more information, see [Creating CloudFront Key Pairs for Your Trusted Signers](#private-content-creating-cloudfront-key-pairs)\.

1. Verify that CloudFront is aware of the new key pairs\. For more information, see [Verifying that Trusted Signers Are Active \(Optional\)](#private-content-verifying-trusted-signers-active)\.

1. Update your application to create signatures using the private keys from the new key pairs\.

1. Confirm that URLs or cookies that you're signing using the new private keys are working\.

1. Wait until the expiration date has passed in URLs or cookies that were signed using the old CloudFront key pairs\.

1. Change the old CloudFront key pairs to **Inactive**:

   1. Sign in to the AWS Management Console using the root credentials for an AWS account for which you want to make key pairs inactive\.

   1. On the *account\-name* menu, click **Security Credentials**\.

   1. Expand **CloudFront Key Pairs**\.

   1. Choose specific key pairs, and then choose **Make Inactive**\.

   1. Repeat steps a through d for each of the AWS accounts for which you want to make key pairs inactive\.

1. Reconfirm that URLs or cookies that you're signing using the new private keys are working\.

1. Delete the old CloudFront key pairs:

   1. Go to the [Your Security Credentials](https://console.aws.amazon.com/iam/home?#security_credential) page\.

   1. Expand **CloudFront Key Pairs**\.

   1. Choose specific key pairs, and then choose **Delete**\.

1. Delete the old private keys from the location where you stored them\.