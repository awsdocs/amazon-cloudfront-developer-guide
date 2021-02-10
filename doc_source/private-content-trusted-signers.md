# Specifying the signers that can create signed URLs and signed cookies<a name="private-content-trusted-signers"></a>

**Topics**
+ [Choosing between trusted key groups \(recommended\) and AWS accounts](#choosing-key-groups-or-AWS-accounts)
+ [Creating key pairs for your signers](#private-content-creating-cloudfront-key-pairs)
+ [Reformatting the private key \(\.NET and Java only\)](#private-content-reformatting-private-key)
+ [Adding a signer to a distribution](#private-content-adding-trusted-signers)
+ [Rotating key pairs](#private-content-rotating-key-pairs)

To create signed URLs or signed cookies, you need a *signer*\. A signer is either a trusted key group that you create in CloudFront, or an AWS account that contains a CloudFront key pair\. We recommend that you use trusted key groups with signed URLs and signed cookies\. For more information, see [Choosing between trusted key groups \(recommended\) and AWS accounts](#choosing-key-groups-or-AWS-accounts)\.

The signer has two purposes:
+ As soon as you add the signer to your distribution, CloudFront starts to require that viewers use signed URLs or signed cookies to access your files\.
+ When you create signed URLs or signed cookies, you use the private key from the signer’s key pair to sign a portion of the URL or the cookie\. When someone requests a restricted file, CloudFront compares the signature in the URL or cookie with the unsigned URL or cookie, to verify that it hasn’t been tampered with\. CloudFront also verifies that the URL or cookie is valid, meaning, for example, that the expiration date and time hasn’t passed\.

When you specify a signer, you also indirectly specify the files that require signed URLs or signed cookies by adding the signer to a cache behavior\. If your distribution has only one cache behavior, viewers must use signed URLs or signed cookies to access any file in the distribution\. If you create multiple cache behaviors and add signers to some cache behaviors and not to others, you can require that viewers use signed URLs or signed cookies to access some files and not others\.

To specify the signers \(the private keys\) that are allowed to create signed URLs or signed cookies, and to add the signers to your CloudFront distribution, do the following tasks:

1. Decide whether to use a trusted key group or an AWS account as the signer\. We recommend using a trusted key group\. For more information, see [Choosing between trusted key groups \(recommended\) and AWS accounts](#choosing-key-groups-or-AWS-accounts)\.

1. For the signer that you chose in step 1, create a public–private key pair\. For more information, see [Creating key pairs for your signers](#private-content-creating-cloudfront-key-pairs)\.

1. If you’re using \.NET or Java to create signed URLs or signed cookies, reformat the private key\. For more information, see [Reformatting the private key \(\.NET and Java only\)](#private-content-reformatting-private-key)\.

1. In the distribution for which you’re creating signed URLs or signed cookies, specify the signer\. For more information, see [Adding a signer to a distribution](#private-content-adding-trusted-signers)\.

## Choosing between trusted key groups \(recommended\) and AWS accounts<a name="choosing-key-groups-or-AWS-accounts"></a>

To use signed URLs or signed cookies, you need a *signer*\. A signer is either a trusted key group that you create in CloudFront, or an AWS account that contains a CloudFront key pair\. We recommend that you use trusted key groups, for the following reasons:
+ With CloudFront key groups, you don’t need to use the AWS account root user to manage the public keys for CloudFront signed URLs and signed cookies\. [AWS best practices](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root) recommend that you don’t use the root user when you don’t have to\.
+ With CloudFront key groups, you can manage public keys, key groups, and trusted signers using the CloudFront API\. You can use the API to automate key creation and key rotation\. When you use the AWS root user, you have to use the AWS Management Console to manage CloudFront key pairs, so you can’t automate the process\.
+ Because you can manage key groups with the CloudFront API, you can also use AWS Identity and Access Management \(IAM\) permissions policies to limit what different users are allowed to do\. For example, you can allow users to upload public keys, but not delete them\. Or you can allow users to delete public keys, but only when certain conditions are met, such as using multi\-factor authentication, sending the request from a particular network, or sending the request within a particular date and time range\.

  When you use the AWS account root user to manage CloudFront key pairs, you can’t restrict what the root user can do or the conditions in which it can do them\. You can’t apply IAM permissions policies to the root user, which is one reason why [AWS best practices recommend against using the root user](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html)\.
+ With CloudFront key groups, you can associate a higher number of public keys with your CloudFront distribution, giving you more flexibility in how you use and manage the public keys\. By default, you can associate up to four key groups with a single distribution, and you can have up to five public keys in a key group\.

  When you use the root user to manage CloudFront key pairs, you can only have up to two active CloudFront key pairs per AWS account\.

## Creating key pairs for your signers<a name="private-content-creating-cloudfront-key-pairs"></a>

Each signer that you use to create CloudFront signed URLs or signed cookies must have a public–private key pair\. The signer uses its private key to sign the URL or cookies, and CloudFront uses the public key to verify the signature\.

The way that you create a key pair depends on whether you use a trusted key group as the signer \(recommended\), or a CloudFront key pair\. For more information, see the following sections\. The key pair that you create must meet the following requirements:
+ It must be an SSH\-2 RSA key pair\.
+ It must be in base64\-encoded PEM format\.
+ It must be a 1024, 2048, or 4096 bits key pair\.

To help secure your applications, we recommend that you rotate key pairs periodically\. For more information, see [Rotating key pairs](#private-content-rotating-key-pairs)\.

### Create a key pair for a trusted key group \(recommended\)<a name="create-key-pair-and-key-group"></a>

To create a key pair for a trusted key group, perform the following steps:

1. Create the public–private key pair\.

1. Upload the public key to CloudFront\.

1. Add the public key to a CloudFront key group\.

For more information, see the following procedures\.<a name="private-content-uploading-cloudfront-public-key-procedure"></a>

**To create a key pair**
**Note**  
The following steps use OpenSSL as an example of one way to create a key pair\. There are many others ways to create an RSA key pair\.

1. The following example command uses OpenSSL to generate an RSA key pair with a length of 2048 bits and save to the file named `private_key.pem`\.

   ```
   openssl genrsa -out private_key.pem 2048
   ```

1. The resulting file contains both the public and the private key\. The following example command extracts the public key from the file named `private_key.pem`\.

   ```
   openssl rsa -pubout -in private_key.pem -out public_key.pem
   ```

   You upload the public key \(in the `public_key.pem` file\) later, in the following procedure\.

**To upload the public key to CloudFront**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation menu, choose **Public keys**\.

1. Choose **Add public key**\.

1. In the **Add public key** window, do the following:

   1. For **Key name**, type a name to identify the public key\.

   1. For **Key value**, paste the public key\. If you followed the steps in the preceding procedure, the public key is in the file named `public_key.pem`\. To copy and paste the contents of the public key, you can:
      + Use the cat command on the macOS or Linux command line, like this:

        ```
        cat public_key.pem
        ```

        Copy the output of that command, then paste it into the **Key value** field\.
      + Open the `public_key.pem` file with a plaintext editor like Notepad \(on Windows\) or TextEdit \(on macOS\)\. Copy the contents of the file, then paste it into the **Key value** field\.

   1. \(Optional\) For **Comment**, add a comment to describe the public key\.

   When finished, choose **Add**\.

1. Record the public key ID\. You use it later when you create signed URLs or signed cookies, as the value of the `Key-Pair-Id` field\.

**To add the public key to a key group**

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation menu, choose **Key groups**\.

1. Choose **Add key group**\.

1. On the **Create key group** page, do the following:

   1. For **Key group name**, type a name to identify the key group\.

   1. \(Optional\) For **Comment**, type a comment to describe the key group\.

   1. For **Public keys**, select the public key to add to the key group, then choose **Add**\. Repeat this step for each public key that you want to add to the key group\.

1. Choose **Create key group**\.

1. Record the key group name\. You use it later to associate the key group with a cache behavior in a CloudFront distribution\. \(In the CloudFront API, you use the key group ID to associate the key group with a cache behavior\.\)

### Create a CloudFront key pair \(not recommended, requires the AWS account root user\)<a name="create-key-pair-aws-account"></a>

**Important**  
We recommend that you create a public key for a trusted key group instead of following these steps\. For the recommended way to create public keys for signed URLs and signed cookies, see [Create a key pair for a trusted key group \(recommended\)](#create-key-pair-and-key-group)\.

You can create a CloudFront key pair in the following ways:
+ Create a key pair in the AWS Management Console and download the private key\. See the following procedure\.
+ Create an RSA key pair by using an application such as OpenSSL, and then upload the public key to the AWS Management Console\. For more information about creating an RSA key pair, see [Create a key pair for a trusted key group \(recommended\)](#create-key-pair-and-key-group)\.<a name="private-content-creating-cloudfront-key-pairs-procedure"></a>

**To create CloudFront key pairs in the AWS Management Console**

1. Sign in to the AWS Management Console using the credentials of the AWS account root user\.
**Important**  
IAM users can’t create CloudFront key pairs\. You must sign in using root user credentials to create key pairs\.

1. Choose your account name, then choose **My Security Credentials**\.

1. Choose **CloudFront key pairs**\.

1. Confirm that you have no more than one active key pair\. You can’t create a key pair if you already have two active key pairs\.

1. Choose **Create New Key Pair**\.

1. In the **Create Key Pair** dialog box, choose **Download Private Key File**, and then save the file on your computer\.
**Important**  
Save the private key for your CloudFront key pair in a secure location, and set permissions on the file so that only the desired administrators can read it\. If someone gets your private key, they can generate valid signed URLs and signed cookies and download your content\. You cannot get the private key again, so if you lose or delete it, you must create a new CloudFront key pair\.

1. Record the key pair ID for your key pair\. \(In the AWS Management Console, this is called the **Access Key ID**\.\) You’ll use it when you create signed URLs or signed cookies\.

## Reformatting the private key \(\.NET and Java only\)<a name="private-content-reformatting-private-key"></a>

If you’re using \.NET or Java to create signed URLs or signed cookies, you cannot use the private key from your key pair in the default PEM format to create the signature\. Instead, do the following:
+ **\.NET framework** – Convert the private key to the XML format that the \.NET framework uses\. Several tools are available\.
+ **Java** – Convert the private key to DER format\. One way to do this is with the following OpenSSL command\. In the following command, `private_key.pem` is the name of the file that contains the PEM\-formatted private key, and `private_key.der` is the name of the file that contains the DER\-formatted private key after you run the command\.

  ```
  openssl pkcs8 -topk8 -nocrypt -in private_key.pem -inform PEM -out private_key.der -outform DER
  ```

  To ensure that the encoder works correctly, add the JAR for the Bouncy Castle Java cryptography APIs to your project and then add the Bouncy Castle provider\.

## Adding a signer to a distribution<a name="private-content-adding-trusted-signers"></a>

A signer is the trusted key group \(recommended\) or CloudFront key pair that can create signed URLs and signed cookies for a distribution\. To use signed URLs or signed cookies with a CloudFront distribution, you must specify a signer\.

Signers are associated with cache behaviors\. This allows you to require signed URLs or signed cookies for some files and not for others in the same distribution\. A distribution requires signed URLs or cookies only for files that are associated with the corresponding cache behaviors\.

Similarly, a signer can only sign URLs or cookies for files that are associated with the corresponding cache behaviors\. For example, if you have one signer for one cache behavior and a different signer for a different cache behavior, neither signer can create signed URLs or cookies for files that are associated with the other cache behavior\.

**Important**  
Before you add a signer to your distribution, do the following:  
Define the path patterns in cache behaviors and the sequence of cache behaviors carefully so you don’t give users unintended access to your content or prevent them from accessing content that you want to be available to everyone\.  
For example, suppose a request matches the path pattern for two cache behaviors\. The first cache behavior does not require signed URLs or signed cookies and the second cache behavior does\. Users will be able to access the files without using signed URLs or signed cookies because CloudFront processes the cache behavior that is associated with the first match\.  
For more information about path patterns, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.
For a distribution that you’re already using to distribute content, make sure you’re ready to start generating signed URLs and signed cookies before you add a signer\. When you add a signer, CloudFront rejects requests that don’t include a valid signed URL or signed cookie\.

You can add signers to your distribution using either the CloudFront console or the CloudFront API\.

**Topics**
+ [Adding a signer to a distribution using the CloudFront console](#private-content-adding-trusted-signers-console)
+ [Adding a signer to a distribution using the CloudFront API](#private-content-adding-trusted-signers-api)

### Adding a signer to a distribution using the CloudFront console<a name="private-content-adding-trusted-signers-console"></a>

The following steps show how to add a trusted key group as a signer\. You can also add an AWS account as a trusted signer, but it’s not recommended\.<a name="private-content-adding-trusted-signers-console-procedure"></a>

**To add a signer to a distribution using the console**

1. Record the key group ID of the key group that you want to use as a trusted signer\. For more information, see [Create a key pair for a trusted key group \(recommended\)](#create-key-pair-and-key-group)\.

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the distribution whose files you want to protect with signed URLs or signed cookies\.
**Note**  
To add a signer to a new distribution, you specify the same settings that are described in step 6 when you create the distribution\.

1. Choose the **Behaviors** tab\.

1. Select the cache behavior whose path pattern matches the files that you want to protect with signed URLs or signed cookies, and then choose **Edit**\.

1. On the **Edit Behavior** page, do the following:

   1. For **Restrict Viewer Access \(Use Signed URLs or Signed Cookies\)**, choose **Yes**\.

   1. For **Trusted Key Groups or Trusted Signer**, choose **Trusted Key Groups**\.

   1. For **Trusted Key Groups**, choose the key group to add, and then choose **Add**\. Repeat if you want to add more than one key group\.

1. Choose **Yes, Edit** to update the cache behavior\.

### Adding a signer to a distribution using the CloudFront API<a name="private-content-adding-trusted-signers-api"></a>

You can use the CloudFront API to add a trusted key group as a signer\. You can add a signer to an existing distribution or to a new distribution\. In either case, specify the values in the `TrustedKeyGroups` element\.

You can also add an AWS account as a trusted signer, but it’s not recommended\.

See the following topics in the *Amazon CloudFront API Reference*:
+ **Update an existing distribution** – [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)
+ **Create a new distribution** – [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)

## Rotating key pairs<a name="private-content-rotating-key-pairs"></a>

We recommend that you periodically rotate \(change\) your key pairs for signed URLs and signed cookies\. To rotate key pairs that you’re using to create signed URLs or signed cookies without invalidating URLs or cookies that haven’t expired yet, do the following tasks:

1. Create a new key pair, and add the public key to a key group\. For more information, see [Create a key pair for a trusted key group \(recommended\)](#create-key-pair-and-key-group)\.

1. If you created a new key group in the previous step, [add the key group to the distribution as a signer](#private-content-adding-trusted-signers-console)\.
**Important**  
Don’t remove any existing public keys from the key group, or any key groups from the distribution yet\. Only add the new ones\.

1. Update your application to create signatures using the private key from the new key pair\. Confirm that the signed URLs or cookies that are signed with the new private keys are working\.

1. Wait until the expiration date has passed in URLs or cookies that were signed using the previous private key\. Then remove the old public key from the key group\. If you created a new key group in step 2, remove the old key group from your distribution\.