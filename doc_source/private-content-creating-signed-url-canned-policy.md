# Creating a Signed URL Using a Canned Policy<a name="private-content-creating-signed-url-canned-policy"></a>

To create a signed URL using a canned policy, do the following procedure\.<a name="private-content-creating-signed-url-canned-policy-procedure"></a>

**To create a signed URL using a canned policy**

1. If you're using \.NET or Java to create signed URLs, and if you haven't reformatted the private key for your key pair from the default \.pem format to a format compatible with \.NET or with Java, do so now\. For more information, see [Reformatting the CloudFront Private Key \(\.NET and Java Only\)](private-content-trusted-signers.md#private-content-reformatting-private-key)\.

1. Concatenate the following values in the specified order, and remove the whitespace \(including tabs and newline characters\) between the parts\. You might have to include escape characters in the string in application code\. All values have a type of String\. Each part is keyed by number \(![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png)\) to the two examples that follow\.  
**![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) *Base URL for the file***  
The base URL is the CloudFront URL that you would use to access the file if you were not using signed URLs, including your own query string parameters, if any\. For more information about the format of URLs for web distributions, see [Customizing the URL Format for Files in CloudFront](LinkFormat.md)\.  
The following examples show values that you specify for web distributions\.  
   + The following CloudFront URL is for a file in a web distribution \(using the CloudFront domain name\)\. Note that `image.jpg` is in an `images` directory\. The path to the file in the URL must match the path to the file on your HTTP server or in your Amazon S3 bucket\.

     `http://d111111abcdef8.cloudfront.net/images/image.jpg`
   + The following CloudFront URL includes a query string:

     `http://d111111abcdef8.cloudfront.net/images/image.jpg?size=large`
   + The following CloudFront URLs are for files in a web distribution\. Both use an alternate domain name; the second one includes a query string:

     `http://www.example.com/images/image.jpg`

     `http://www.example.com/images/image.jpg?color=red`
   + The following CloudFront URL is for files in a web distribution that uses an alternate domain name and the HTTPS protocol:

     `https://www.example.com/images/image.jpg`
For RTMP distributions, the following examples are for files in two different video formats, MP4 and FLV:  
   + **MP4** – `mp4:sydney-vacation.mp4`
   + **FLV** – `sydney-vacation`
   + **FLV** – `sydney-vacation.flv`
For \.flv files, whether you include the `.flv` filename extension depends on your player\. To serve MP3 audio files or H\.264/MPEG\-4 video files, you might need to prefix the file name with `mp3:` or `mp4:`\. Some media players can be configured to add the prefix automatically\. The media player might also require you to specify the file name without the file extension \(for example, sydney\-vacation instead of sydney\-vacation\.mp4\)\.  
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
Specify the expiration date and time in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\. To use epoch time, use a 32\-bit integer for a date which can be no later than 2147483647 \(January 19th, 2038 at 03:14:07 UTC\)\. For information about UTC, see *RFC 3339, Date and Time on the Internet: Timestamps*, [http://tools\.ietf\.org/html/rfc3339](http://tools.ietf.org/html/rfc3339)\.  
**![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=`*hashed and signed version of the policy statement***  
A hashed, signed, and base64\-encoded version of the JSON policy statement\. For more information, see [Creating a Signature for a Signed URL That Uses a Canned Policy](#private-content-canned-policy-creating-signature)\.  
**![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=`*active CloudFront key pair Id for the key pair that you're using to generate the signature***  
The ID for an active CloudFront key pair, for example, APKA9ONS7QCOWEXAMPLE\. The CloudFront key pair ID tells CloudFront which public key to use to validate the signed URL\. CloudFront compares the information in the signature with the information in the policy statement to verify that the URL has not been tampered with\.  
The key pair ID that you include in CloudFront signed URLs must be the ID of an active key pair for one of your trusted signers:  
   + **Web distributions** – The key pair must be associated with an AWS account that is one of the trusted signers for the cache behavior\.
   + **RTMP distributions** – The key pair must be associated with an AWS account that is one of the trusted signers for the distribution\.
For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.  
If you make a key pair inactive while rotating CloudFront key pairs, and if you're generating signed URLs programmatically, you must update your application to use a new active key pair for one of your trusted signers\. If you're generating signed URLs manually, you must create new signed URLs\. For more information about rotating key pairs, see [Rotating CloudFront Key Pairs](private-content-trusted-signers.md#private-content-rotating-key-pairs)\.

Example signed URL for a web distribution:

** ![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) `http://d111111abcdef8.cloudfront.net/image.jpg ` ![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) `? ` ![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) `color=red&size=medium& ` ![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) `Expires=1357034400 ` ![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=nitfHRCrtziwO2HwPfWw~yYDhUF5EwRunQA-j19DzZrvDh6hQ73lDx~-ar3UocvvRQVw6EkC~GdpGQyyOSKQim-TxAnW7d8F5Kkai9HVx0FIu-5jcQb0UEmatEXAMPLE3ReXySpLSMj0yCd3ZAB4UcBCAqEijkytL6f3fVYNGQI6 ` ![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=APKA9ONS7QCOWEXAMPLE` **

Example signed URL for an RTMP distribution:

** ![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) `videos/mediafile.flv ` ![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) `? ` ![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) `color=red&size=medium& ` ![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) `Expires=1357034400 ` ![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=nitfHRCrtziwO2HwPfWw~yYDhUF5EwRunQA-j19DzZrvDh6hQ73lDx~-ar3UocvvRQVw6EkC~GdpGQyyOSKQim-TxAnW7d8F5Kkai9HVx0FIu-5jcQb0UEmatEXAMPLE3ReXySpLSMj0yCd3ZAB4UcBCAqEijkytL6f3fVYNGQI6 ` ![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=APKA9ONS7QCOWEXAMPLE` **

## Creating a Signature for a Signed URL That Uses a Canned Policy<a name="private-content-canned-policy-creating-signature"></a>

To create the signature for a signed URL that uses a canned policy, you do the following procedures:

1. Create a policy statement\. See [Creating a Policy Statement for a Signed URL That Uses a Canned Policy](#private-content-canned-policy-creating-policy-statement)\.

1. Sign the policy statement to create a signature\. See [Creating a Signature for a Signed URL That Uses a Canned Policy](#private-content-canned-policy-signing-policy-statement)\.

### Creating a Policy Statement for a Signed URL That Uses a Canned Policy<a name="private-content-canned-policy-creating-policy-statement"></a>

When you create a signed URL using a canned policy, the `Signature` parameter is a hashed and signed version of a policy statement\. For signed URLs that use a canned policy, you don't include the policy statement in the URL, as you do for signed URLs that use a custom policy\. To create the policy statement, do the following procedure\.<a name="private-content-canned-policy-creating-policy-statement-procedure"></a>

**To create the policy statement for a signed URL that uses a canned policy**

1. Construct the policy statement using the following JSON format and using UTF\-8 character encoding\. Include all punctuation and other literal values exactly as specified\. For information about the `Resource` and `DateLessThan` parameters, see [Values that You Specify in the Policy Statement for a Signed URL That Uses a Canned Policy](#private-content-canned-policy-statement-values)\.

   ```
   {
      "Statement":[
         {
            "Resource":"base URL or stream name",
            "Condition":{
               "DateLessThan":{
                  "AWS:EpochTime":ending date and time in Unix time format and UTC
               }
            }
         }
      ]
   }
   ```

1. Remove all whitespace \(including tabs and newline characters\) from the policy statement\. You might have to include escape characters in the string in application code\.

#### Values that You Specify in the Policy Statement for a Signed URL That Uses a Canned Policy<a name="private-content-canned-policy-statement-values"></a>

When you create a policy statement for a canned policy, you specify the following values\.

**Resource**  
The value that you specify depends on whether you're creating the signed URL for a web distribution or an RTMP distribution\.  
You can specify only one value for `Resource`\.  
**Web distributions**  
The base URL including your query strings, if any, but excluding the CloudFront `Expires`, `Signature`, and `Key-Pair-Id` parameters, for example:  
`http://d111111abcdef8.cloudfront.net/images/horizon.jpg?size=large&license=yes`  
Note the following:  
+ **Protocol** – The value must begin with `http://` or `https://`\. 
+ **Query string parameters** – If you have no query string parameters, omit the question mark\.
+ **Alternate domain names** – If you specify an alternate domain name \(CNAME\) in the URL, you must specify the alternate domain name when referencing the file in your web page or application\. Do not specify the Amazon S3 URL for the object\.  
**RTMP distributions**  
Include only the stream name\. For example, if the full URL for a streaming video is:  
`rtmp://s5c39gqb8ow64r.cloudfront.net/videos/cfx/st/mp3_name.mp3`  
then use the following value for `Resource`:  
`videos/mp3_name`  
Do not include a prefix such as `mp3:` or `mp4:`\. Also, depending on the player you're using, you might have to omit the file extension from the value of `Resource`\. For example, you might need to use `sydney-vacation` instead of `sydney-vacation.flv`\.

**DateLessThan**  
The expiration date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\.  
This value must match the value of the `Expires` query string parameter in the signed URL\. Do not enclose the value in quotation marks\.  
For more information, see [When Does CloudFront Check the Expiration Date and Time in a Signed URL?](private-content-signed-urls.md#private-content-check-expiration)\.

#### Example Policy Statement for a Signed URL That Uses a Canned Policy<a name="private-content-canned-policy-creating-policy-statement-example"></a>

When you use the following example policy statement in a signed URL, a user can access the file `http://d111111abcdef8.cloudfront.net/horizon.jpg` until January 1, 2013 10:00 am UTC:

```
{
   "Statement":[
      {
         "Resource":"http://d111111abcdef8.cloudfront.net/horizon.jpg?size=large&license=yes",
         "Condition":{
            "DateLessThan":{
               "AWS:EpochTime":1357034400
            }
         }
      }
   ]
}
```

### Creating a Signature for a Signed URL That Uses a Canned Policy<a name="private-content-canned-policy-signing-policy-statement"></a>

To create the value for the `Signature` parameter in a signed URL, you hash and sign the policy statement that you created in [Creating a Policy Statement for a Signed URL That Uses a Canned Policy](#private-content-canned-policy-creating-policy-statement)\. There are two versions of this procedure\. Follow the procedure for your scenario:
+ [Option 1: To create a signature for a web distribution or for an RTMP distribution \(without Adobe Flash Player\) by using a canned policy](#private-content-canned-policy-creating-signature-download-procedure)
+ [Option 2: To create a signature for an RTMP distribution by using a canned policy \(Adobe Flash Player\)](#private-content-canned-policy-creating-signature-streaming-flash-procedure)

For additional information and examples of how to hash, sign, and encode the policy statement, see:
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)<a name="private-content-canned-policy-creating-signature-download-procedure"></a>

**Option 1: To create a signature for a web distribution or for an RTMP distribution \(without Adobe Flash Player\) by using a canned policy**

1. Use the SHA\-1 hash function and RSA to hash and sign the policy statement that you created in the procedure [To create the policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-creating-policy-statement-procedure)\. Use the version of the policy statement that no longer includes whitespace\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-canned-policy.html)

1. Append the resulting value to your signed URL after `&Signature=`, and return to [To create a signed URL using a canned policy](#private-content-creating-signed-url-canned-policy-procedure) to finish concatenating the parts of your signed URL\.<a name="private-content-canned-policy-creating-signature-streaming-flash-procedure"></a>

**Option 2: To create a signature for an RTMP distribution by using a canned policy \(Adobe Flash Player\)**

1. Use the SHA\-1 hash function and RSA to hash and sign the policy statement that you created in the [To create the policy statement for a signed URL that uses a canned policy](#private-content-canned-policy-creating-policy-statement-procedure) procedure\. Use the version of the policy statement that no longer includes whitespace\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

   Continue on to Step 3 if you're using Adobe Flash Player and the stream name is passed in from a web page\.

   If you're using Adobe Flash Player and if the stream name is not passed in from a web page, skip the rest of this procedure\. For example, if you wrote your own player that fetches stream names from within the Adobe Flash \.swf file, skip the rest of this procedure\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-canned-policy.html)

1. Some versions of Adobe Flash Player require that you URL\-encode the characters ?, =, and &\. For information about whether your version of Adobe Flash Player requires this character substitution, refer to the Adobe website\.

   If your version of Flash does not require URL\-encoding those character, skip to Step 6\.

   If your version of Flash requires URL\-encoding those characters, replace them as indicated in the following table\. \(You already replaced = in the previous step\.\)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-canned-policy.html)

1. Append the resulting value to your signed URL after `&Signature=`, and return to [To create a signed URL using a canned policy](#private-content-creating-signed-url-canned-policy-procedure) to finish concatenating the parts of your signed URL\.