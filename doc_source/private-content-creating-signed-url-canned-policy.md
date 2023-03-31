# Creating a signed URL using a canned policy<a name="private-content-creating-signed-url-canned-policy"></a>

To create a signed URL using a canned policy, complete the following steps\.<a name="private-content-creating-signed-url-canned-policy-procedure"></a>

**To create a signed URL using a canned policy**

1. If you're using \.NET or Java to create signed URLs, and if you haven't reformatted the private key for your key pair from the default \.pem format to a format compatible with \.NET or with Java, do so now\. For more information, see [Reformatting the private key \(\.NET and Java only\)](private-content-trusted-signers.md#private-content-reformatting-private-key)\.

1. Concatenate the following values in the specified order, and remove the white space \(including tabs and newline characters\) between the parts\. You might have to include escape characters in the string in application code\. All values have a type of String\. Each part is keyed by number \(![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png)\) to the two examples that follow\.  
**![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) *Base URL for the file***  
The base URL is the CloudFront URL that you would use to access the file if you were not using signed URLs, including your own query string parameters, if any\. For more information about the format of URLs for distributions, see [Customizing the URL format for files in CloudFront](LinkFormat.md)\.  
   + The following CloudFront URL is for an image file in a distribution \(using the CloudFront domain name\)\. Note that `image.jpg` is in an `images` directory\. The path to the file in the URL must match the path to the file on your HTTP server or in your Amazon S3 bucket\.

     `https://d111111abcdef8.cloudfront.net/images/image.jpg`
   + The following CloudFront URL includes a query string:

     `https://d111111abcdef8.cloudfront.net/images/image.jpg?size=large`
   + The following CloudFront URLs are for image files in a distribution\. Both use an alternate domain name; the second one includes a query string:

     `https://www.example.com/images/image.jpg`

     `https://www.example.com/images/image.jpg?color=red`
   + The following CloudFront URL is for an image file in a distribution that uses an alternate domain name and the HTTPS protocol:

     `https://www.example.com/images/image.jpg`  
**![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) **?****  
The **?** indicates that query string parameters follow the base URL\. Include the **?** even if you don't have any query string parameters of your own\.  
**![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) *Your query string parameters, if any***&****  
This value is optional\. If you want to add your own query string parameters, for example:  
`color=red&size=medium`  
then add the parameters after the **?** \(see ![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png)\) and before the `Expires` parameter\. In certain rare circumstances, you might need to put your query string parameters after `Key-Pair-Id`\.  
Your parameters cannot be named `Expires`, `Signature`, or `Key-Pair-Id`\.
If you add your own parameters, append an **&** after each one, including the last one\.  
**![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) `Expires=`*date and time in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)***  
The date and time that you want the URL to stop allowing access to the file\.  
Specify the expiration date and time in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\. To use epoch time, use a 32\-bit integer for a date that's no later than 2147483647 \(January 19th, 2038 at 03:14:07 UTC\)\. For information about UTC, see *RFC 3339, Date and Time on the Internet: Timestamps*, [https://tools.ietf.org/html/rfc3339](https://tools.ietf.org/html/rfc3339)\.  
**![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=`*hashed and signed version of the policy statement***  
A hashed, signed, and base64\-encoded version of the JSON policy statement\. For more information, see [Creating a signature for a signed URL that uses a canned policy](#private-content-canned-policy-creating-signature)\.  
**![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=`*public key ID for the CloudFront public key whose corresponding private key you're using to generate the signature***  
The ID for a CloudFront public key, for example, `K2JCJMDEHXQW5F`\. The public key ID tells CloudFront which public key to use to validate the signed URL\. CloudFront compares the information in the signature with the information in the policy statement to verify that the URL has not been tampered with\.  
This public key must belong to a key group that is a trusted signer in the distribution\. For more information, see [Specifying the signers that can create signed URLs and signed cookies](private-content-trusted-signers.md)\.

Example signed URL:

** ![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) `https://d111111abcdef8.cloudfront.net/image.jpg ` ![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) `? ` ![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) `color=red&size=medium& ` ![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) `Expires=1357034400 ` ![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=nitfHRCrtziwO2HwPfWw~yYDhUF5EwRunQA-j19DzZrvDh6hQ73lDx~-ar3UocvvRQVw6EkC~GdpGQyyOSKQim-TxAnW7d8F5Kkai9HVx0FIu-5jcQb0UEmatEXAMPLE3ReXySpLSMj0yCd3ZAB4UcBCAqEijkytL6f3fVYNGQI6 ` ![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=K2JCJMDEHXQW5F`**

## Creating a signature for a signed URL that uses a canned policy<a name="private-content-canned-policy-creating-signature"></a>

To create the signature for a signed URL that uses a canned policy, you do the following procedures:

1. Create a policy statement\. See [Creating a policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-creating-policy-statement)\.

1. Sign the policy statement to create a signature\. See [Creating a signature for a signed URL that uses a canned policy](#private-content-canned-policy-signing-policy-statement)\.

### Creating a policy statement for a signed URL that uses a canned policy<a name="private-content-canned-policy-creating-policy-statement"></a>

When you create a signed URL using a canned policy, the `Signature` parameter is a hashed and signed version of a policy statement\. For signed URLs that use a canned policy, you don't include the policy statement in the URL, as you do for signed URLs that use a custom policy\. To create the policy statement, do the following procedure\.<a name="private-content-canned-policy-creating-policy-statement-procedure"></a>

**To create the policy statement for a signed URL that uses a canned policy**

1. Construct the policy statement using the following JSON format and using UTF\-8 character encoding\. Include all punctuation and other literal values exactly as specified\. For information about the `Resource` and `DateLessThan` parameters, see [Values that you specify in the policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-statement-values)\.

   ```
   {
       "Statement": [
           {
               "Resource": "base URL or stream name",
               "Condition": {
                   "DateLessThan": {
                       "AWS:EpochTime": ending date and time in Unix time format and UTC
                   }
               }
           }
       ]
   }
   ```

1. Remove all white space \(including tabs and newline characters\) from the policy statement\. You might have to include escape characters in the string in application code\.

#### Values that you specify in the policy statement for a signed URL that uses a canned policy<a name="private-content-canned-policy-statement-values"></a>

When you create a policy statement for a canned policy, you specify the following values\.

**Resource**  
You can specify only one value for `Resource`\.
The base URL including your query strings, if any, but excluding the CloudFront `Expires`, `Signature`, and `Key-Pair-Id` parameters, for example:  
`https://d111111abcdef8.cloudfront.net/images/horizon.jpg?size=large&license=yes`  
Note the following:  
+ **Protocol** – The value must begin with `http://` or `https://`\.
+ **Query string parameters** – If you have no query string parameters, omit the question mark\.
+ **Alternate domain names** – If you specify an alternate domain name \(CNAME\) in the URL, you must specify the alternate domain name when referencing the file in your webpage or application\. Do not specify the Amazon S3 URL for the object\.

**DateLessThan**  
The expiration date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\.  
This value must match the value of the `Expires` query string parameter in the signed URL\. Do not enclose the value in quotation marks\.  
For more information, see [When does CloudFront check the expiration date and time in a signed URL?](private-content-signed-urls.md#private-content-check-expiration)\.

#### Example policy statement for a signed URL that uses a canned policy<a name="private-content-canned-policy-creating-policy-statement-example"></a>

When you use the following example policy statement in a signed URL, a user can access the file `https://d111111abcdef8.cloudfront.net/horizon.jpg` until January 1, 2013 10:00 am UTC:

```
{
    "Statement": [
        {
            "Resource": "https://d111111abcdef8.cloudfront.net/horizon.jpg?size=large&license=yes",
            "Condition": {
                "DateLessThan": {
                    "AWS:EpochTime": 1357034400
                }
            }
        }
    ]
}
```

### Creating a signature for a signed URL that uses a canned policy<a name="private-content-canned-policy-signing-policy-statement"></a>

To create the value for the `Signature` parameter in a signed URL, you hash and sign the policy statement that you created in [Creating a policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-creating-policy-statement)\.

For additional information and examples of how to hash, sign, and encode the policy statement, see:
+ [Using Linux commands and OpenSSL for base64 encoding and encryption](private-content-linux-openssl.md)
+ [Code examples for creating a signature for a signed URL](PrivateCFSignatureCodeAndExamples.md)<a name="private-content-canned-policy-creating-signature-download-procedure"></a>

**Option 1: To create a signature by using a canned policy**

1. Use the SHA\-1 hash function and RSA to hash and sign the policy statement that you created in the procedure [To create the policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-creating-policy-statement-procedure)\. Use the version of the policy statement that no longer includes white space\.

   For the private key that is required by the hash function, use a private key whose public key is in an active trusted key group for the distribution\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code examples for creating a signature for a signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove white space \(including tabs and newline characters\) from the hashed and signed string\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](https://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-canned-policy.html)

1. Append the resulting value to your signed URL after `&Signature=`, and return to [To create a signed URL using a canned policy](#private-content-creating-signed-url-canned-policy-procedure) to finish concatenating the parts of your signed URL\.