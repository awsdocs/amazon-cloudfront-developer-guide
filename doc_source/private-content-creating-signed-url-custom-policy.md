# Creating a Signed URL Using a Custom Policy<a name="private-content-creating-signed-url-custom-policy"></a>

**Topics**
+ [Creating a Policy Statement for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement)
+ [Example Policy Statements for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement-examples)
+ [Creating a Signature for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-creating-signature)

To create a signed URL using a custom policy, do the following procedure\.<a name="private-content-creating-signed-url-custom-policy-procedure"></a>

**To create a signed URL using a custom policy**

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
then add them after the **?** \(see ![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png)\) and before the `Policy` parameter\. In certain rare circumstances, you might need to put your query string parameters after `Key-Pair-Id`\.  
Your parameters cannot be named `Policy`, `Signature`, or `Key-Pair-Id`\.
If you add your own parameters, append an **&** after each one, including the last one\.   
**![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) `Policy=`*base64 encoded version of policy statement***  
Your policy statement in JSON format, with white space removed, then base64 encoded\. For more information, see [Creating a Policy Statement for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement)\.  
The policy statement controls the access that a signed URL grants to a user: the URL of the file \(for web distributions\) or the stream name \(for RTMP distributions\), an expiration date and time, an optional date and time that the URL becomes valid, and an optional IP address or range of IP addresses that are allowed to access the file\.  
**![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `&Signature=`*hashed and signed version of the policy statement***  
A hashed, signed, and base64\-encoded version of the JSON policy statement\. For more information, see [Creating a Signature for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-creating-signature)\.  
**![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png) `&Key-Pair-Id=`*active CloudFront key pair Id for the key pair that you're using to sign the policy statement***  
The ID for an active CloudFront key pair, for example, APKA9ONS7QCOWEXAMPLE\. The CloudFront key pair ID tells CloudFront which public key to use to validate the signed URL\. CloudFront compares the information in the signature with the information in the policy statement to verify that the URL has not been tampered with\.  
The key pair ID that you include in CloudFront signed URLs must be the ID of an active key pair for one of your trusted signers:  
   + **Web distributions** – The key pair must be associated with an AWS account that is one of the trusted signers for the cache behavior\.
   + **RTMP distributions** – The key pair must be associated with an AWS account that is one of the trusted signers for the distribution\.
For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.  
If you make a key pair inactive while rotating CloudFront key pairs, and if you're generating signed URLs programmatically, you must update your application to use a new active key pair for one of your trusted signers\. If you're generating signed URLs manually, you must create new signed URLs\. For more information about rotating key pairs, see [Rotating CloudFront Key Pairs](private-content-trusted-signers.md#private-content-rotating-key-pairs)\.

Example signed URL for a web distribution:

** ![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png)`http://d111111abcdef8.cloudfront.net/image.jpg `![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png)`? `![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png)`color=red&size=medium& `![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png)`Policy=eyANCiAgICEXAMPLEW1lbnQiOiBbeyANCiAgICAgICJSZXNvdXJjZSI6Imh0dHA 6Ly9kemJlc3FtN3VuMW0wLmNsb3VkZnJvbnQubmV0L2RlbW8ucGhwIiwgDQogICAgICAiQ 29uZGl0aW9uIjp7IA0KICAgICAgICAgIklwQWRkcmVzcyI6eyJBV1M6U291cmNlSXAiOiI yMDcuMTcxLjE4MC4xMDEvMzIifSwNCiAgICAgICAgICJEYXRlR3JlYXRlclRoYW4iOnsiQ VdTOkVwb2NoVGltZSI6MTI5Njg2MDE3Nn0sDQogICAgICAgICAiRGF0ZUxlc3NUaGFuIjp 7IkFXUzpFcG9jaFRpbWUiOjEyOTY4NjAyMjZ9DQogICAgICB9IA0KICAgfV0gDQp9DQo `![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png)`&Signature=nitfHRCrtziwO2HwPfWw~yYDhUF5EwRunQA-j19DzZrvDh6hQ73lDx~ -ar3UocvvRQVw6EkC~GdpGQyyOSKQim-TxAnW7d8F5Kkai9HVx0FIu-5jcQb0UEmat EXAMPLE3ReXySpLSMj0yCd3ZAB4UcBCAqEijkytL6f3fVYNGQI6 `![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png)`&Key-Pair-Id=APKA9ONS7QCOWEXAMPLE`**

Example signed URL for an RTMP distribution:

** ![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png)`videos/mediafile.flv `![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png)`? `![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png)`color=red&size=medium& `![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png)`Policy=eyANCiAgICEXAMPLEW1lbnQiOiBbeyANCiAgICAgICJSZXNvdXJjZSI6Imh0dHA 6Ly9kemJlc3FtN3VuMW0wLmNsb3VkZnJvbnQubmV0L2RlbW8ucGhwIiwgDQogICAgICAiQ 29uZGl0aW9uIjp7IA0KICAgICAgICAgIklwQWRkcmVzcyI6eyJBV1M6U291cmNlSXAiOiI yMDcuMTcxLjE4MC4xMDEvMzIifSwNCiAgICAgICAgICJEYXRlR3JlYXRlclRoYW4iOnsiQ VdTOkVwb2NoVGltZSI6MTI5Njg2MDE3Nn0sDQogICAgICAgICAiRGF0ZUxlc3NUaGFuIjp 7IkFXUzpFcG9jaFRpbWUiOjEyOTY4NjAyMjZ9DQogICAgICB9IA0KICAgfV0gDQp9DQo `![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png)`&Signature=nitfHRCrtziwO2HwPfWw~yYDhUF5EwRunQA-j19DzZrvDh6hQ73lDx~ -ar3UocvvRQVw6EkC~GdpGQyyOSKQim-TxAnW7d8F5Kkai9HVx0FIu-5jcQb0UEmat EXAMPLE3ReXySpLSMj0yCd3ZAB4UcBCAqEijkytL6f3fVYNGQI6 `![\[6\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/6.png)`&Key-Pair-Id=APKA9ONS7QCOWEXAMPLE`**

## Creating a Policy Statement for a Signed URL That Uses a Custom Policy<a name="private-content-custom-policy-statement"></a>

To create a policy statement for a custom policy, do the following procedure\. For several example policy statements that control access to files in a variety of ways, see [Example Policy Statements for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement-examples)\.<a name="private-content-custom-policy-creating-policy-procedure"></a>

**To create the policy statement for a signed URL that uses a custom policy**

1. Construct the policy statement using the following JSON format\. For more information, see [Values that You Specify in the Policy Statement for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement-values)\.

   ```
   {
      "Statement": [
         {
            "Resource":"URL or stream name of the file",
            "Condition":{
               "DateLessThan":{"AWS:EpochTime":required ending date and time in Unix time format and UTC},
               "DateGreaterThan":{"AWS:EpochTime":optional beginning date and time in Unix time format and UTC},
               "IpAddress":{"AWS:SourceIp":"optional IP address"}
            }
         }
      ]
   }
   ```

   Note the following:
   + You can include only one statement\.
   + Use UTF\-8 character encoding\.
   + Include all punctuation and parameter names exactly as specified\. Abbreviations for parameter names are not accepted\.
   + The order of the parameters in the `Condition` section doesn't matter\.
   + For information about the values for `Resource`, `DateLessThan`, `DateGreaterThan`, and `IpAddress`, see [Values that You Specify in the Policy Statement for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement-values)\.

1. Remove all whitespace \(including tabs and newline characters\) from the policy statement\. You might have to include escape characters in the string in application code\.

1. Base64\-encode the policy statement using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)

1. Append the resulting value to your signed URL after `Policy=`\.

1. Create a signature for the signed URL by hashing, signing, and base64\-encoding the policy statement\. For more information, see [Creating a Signature for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-creating-signature)\.

### Values that You Specify in the Policy Statement for a Signed URL That Uses a Custom Policy<a name="private-content-custom-policy-statement-values"></a>

When you create a policy statement for a custom policy, you specify the following values\.

**Resource**  
The value that you specify depends on whether you're creating signed URLs for web or RTMP distributions\.  
You can specify only one value for `Resource`\.  
**Web distributions \(optional but recommended\)**  
The base URL including your query strings, if any, but excluding the CloudFront `Policy`, `Signature`, and `Key-Pair-Id` parameters, for example:  
`http://d111111abcdef8.cloudfront.net/images/horizon.jpg?size=large&license=yes`  
If you omit the Resource parameter for a web distribution, users can access all of the files associated with any distribution that is associated with the key pair that you use to create the signed URL\.
Note the following:  
+ **Protocol** – The value must begin with `http://`, `https://`, or `*`\. 
+ **Query string parameters** – If you have no query string parameters, omit the question mark\.
+ **Wildcard characters** – You can use the wildcard character that matches zero or more characters \(\*\) or the wild\-card character that matches exactly one character \(?\) anywhere in the string\. For example, the value:

  `http://d111111abcdef8.cloudfront.net/*game_download.zip*`

  would include \(for example\) the following files:
  + `http://d111111abcdef8.cloudfront.net/game_download.zip`
  + `http://d111111abcdef8.cloudfront.net/example_game_download.zip?license=yes`
  + `http://d111111abcdef8.cloudfront.net/test_game_download.zip?license=temp`
+ **Alternate domain names** – If you specify an alternate domain name \(CNAME\) in the URL, you must specify the alternate domain name when referencing the file in your web page or application\. Do not specify the Amazon S3 URL for the file\.  
**RTMP distributions**  
Include only the stream name\. For example, if the full URL for a streaming video is:  
`rtmp://s5c39gqb8ow64r.cloudfront.net/videos/cfx/st/mp3_name.mp3`  
then use the following value for `Resource`:  
`videos/mp3_name`  
Do not include a prefix such as `mp3:` or `mp4:`\. Also, depending on the player you're using, you might have to omit the file extension from the value of `Resource`\. For example, you might need to use `sydney-vacation` instead of `sydney-vacation.flv`\.

**DateLessThan**  
The expiration date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. Do not enclose the value in quotation marks\. For information about UTC, see *RFC 3339, Date and Time on the Internet: Timestamps*, [http://tools\.ietf\.org/html/rfc3339](http://tools.ietf.org/html/rfc3339)\.  
For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\.   
This is the only required parameter in the `Condition` section\. CloudFront requires this value to prevent users from having permanent access to your private content\.  
For more information, see [When Does CloudFront Check the Expiration Date and Time in a Signed URL?](private-content-signed-urls.md#private-content-check-expiration)

**DateGreaterThan \(Optional\)**  
An optional start date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. Users are not allowed to access the file before the specified date and time\. Do not enclose the value in quotation marks\. 

**IpAddress \(Optional\)**  
The IP address of the client making the GET request\. Note the following:  
+ To allow any IP address to access the file, omit the `IpAddress` parameter\.
+ You can specify either one IP address or one IP address range\. For example, you can't set the policy to allow access if the client's IP address is in one of two separate ranges\.
+ To allow access from a single IP address, you specify:

  `"`*IPv4 IP address*`/32"`
+ You must specify IP address ranges in standard IPv4 CIDR format \(for example, `192.0.2.0/24`\)\. For more information, see *RFC 4632, Classless Inter\-domain Routing \(CIDR\): The Internet Address Assignment and Aggregation Plan*, [http://tools\.ietf\.org/html/rfc4632](http://tools.ietf.org/html/rfc4632)\.
**Important**  
IP addresses in IPv6 format, such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334, are not supported\. 

  If you're using a custom policy that includes `IpAddress`, do not enable IPv6 for the distribution\. If you want to restrict access to some content by IP address and support IPv6 requests for other content, you can create two distributions\. For more information, see [Enable IPv6](distribution-web-values-specify.md#DownloadDistValuesEnableIPv6) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Example Policy Statements for a Signed URL That Uses a Custom Policy<a name="private-content-custom-policy-statement-examples"></a>

The following example policy statements show how to control access to a specific file, all of the files in a directory, or all of the files associated with a key pair ID\. The examples also show how to control access from an individual IP address or a range of IP addresses, and how to prevent users from using the signed URL after a specified date and time\.

If you copy and paste any of these examples, remove any whitespace \(including tabs and newline characters\), replace the values with your own values, and include a newline character after the closing brace \( \} \)\.

For more information, see [Values that You Specify in the Policy Statement for a Signed URL That Uses a Custom Policy](#private-content-custom-policy-statement-values)\.

**Topics**
+ [Example Policy Statement: Accessing One File from a Range of IP Addresses](#private-content-custom-policy-statement-example-one-object)
+ [Example Policy Statement: Accessing All Files in a Directory from a Range of IP Addresses](#private-content-custom-policy-statement-example-all-objects)
+ [Example Policy Statement: Accessing All Files Associated with a Key Pair ID from One IP Address](#private-content-custom-policy-statement-example-one-ip)

### Example Policy Statement: Accessing One File from a Range of IP Addresses<a name="private-content-custom-policy-statement-example-one-object"></a>

The following example custom policy in a signed URL specifies that a user can access the file `http://d111111abcdef8.cloudfront.net/game_download.zip` from IP addresses in the range `192.0.2.0/24` until January 1, 2013 10:00 am UTC:

```
{
   "Statement": [
      {
         "Resource":"http://d111111abcdef8.cloudfront.net/game_download.zip",
         "Condition":{
            "IpAddress":{"AWS:SourceIp":"192.0.2.0/24"},
            "DateLessThan":{"AWS:EpochTime":1357034400}
         }
      }
   ]
}
```

### Example Policy Statement: Accessing All Files in a Directory from a Range of IP Addresses<a name="private-content-custom-policy-statement-example-all-objects"></a>

The following example custom policy allows you to create signed URLs for any file in the `training` directory, as indicated by the \* wildcard character in the `Resource` parameter\. Users can access the file from an IP address in the range `192.0.2.0/24` until January 1, 2013 10:00 am UTC:

```
{ 
   "Statement": [
      { 
         "Resource":"http://d111111abcdef8.cloudfront.net/training/*", 
         "Condition":{ 
            "IpAddress":{"AWS:SourceIp":"192.0.2.0/24"}, 
            "DateLessThan":{"AWS:EpochTime":1357034400}
         }
      }
   ] 
}
```

Each signed URL in which you use this policy includes a base URL that identifies a specific file, for example:

`http://d111111abcdef8.cloudfront.net/training/orientation.pdf`

### Example Policy Statement: Accessing All Files Associated with a Key Pair ID from One IP Address<a name="private-content-custom-policy-statement-example-one-ip"></a>

The following sample custom policy allows you to create signed URLs for any file associated with any distribution, as indicated by the \* wildcard character in the `Resource` parameter\. The user must use the IP address `192.0.2.10/32`\. \(The value `192.0.2.10/32` in CIDR notation refers to a single IP address, `192.0.2.10`\.\) The files are available only from January 1, 2013 10:00 am UTC until January 2, 2013 10:00 am UTC:

```
{ 
   "Statement": [
      { 
         "Resource":"http://*",
         "Condition":{ 
            "IpAddress":{"AWS:SourceIp":"192.0.2.10/32"},
            "DateGreaterThan":{"AWS:EpochTime":1357034400},
            "DateLessThan":{"AWS:EpochTime":1357120800}
         } 
      }
   ] 
}
```

Each signed URL in which you use this policy includes a base URL that identifies a specific file in a specific CloudFront distribution, for example:

`http://d111111abcdef8.cloudfront.net/training/orientation.pdf`

The signed URL also includes a key pair ID, which must be associated with a trusted signer in the distribution \(d111111abcdef8\.cloudfront\.net\) that you specify in the base URL\.

## Creating a Signature for a Signed URL That Uses a Custom Policy<a name="private-content-custom-policy-creating-signature"></a>

The signature for a signed URL that uses a custom policy is a hashed, signed, and base64\-encoded version of the policy statement\. To create a signature for a custom policy, follow the procedure for your scenario\. The version that you choose depends on your distribution type \(web or RTMP\) and, for RTMP distributions, the media player that you're using \(Adobe Flash Player or another media player\):
+ [Option 1: To create a signature for a web distribution or for an RTMP distribution \(without Adobe Flash Player\) by using a custom policy](#private-content-custom-policy-creating-signature-download-procedure)
+ [Option 2: To create a signature for an RTMP distribution by using a custom policy \(Adobe Flash Player\)](#private-content-custom-policy-creating-signature-streaming-flash-procedure)

For additional information and examples of how to hash, sign, and encode the policy statement, see:
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)<a name="private-content-custom-policy-creating-signature-download-procedure"></a>

**Option 1: To create a signature for a web distribution or for an RTMP distribution \(without Adobe Flash Player\) by using a custom policy**

1. Use the SHA\-1 hash function and RSA to hash and sign the JSON policy statement that you created in the procedure [To create the policy statement for a signed URL that uses a custom policy](#private-content-custom-policy-creating-policy-procedure)\. Use the version of the policy statement that no longer includes whitespace but that has not yet been base64\-encoded\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)

1. Append the resulting value to your signed URL after `&Signature=`, and return to [To create a signed URL using a custom policy](#private-content-creating-signed-url-custom-policy-procedure) to finish concatenating the parts of your signed URL\.<a name="private-content-custom-policy-creating-signature-streaming-flash-procedure"></a>

**Option 2: To create a signature for an RTMP distribution by using a custom policy \(Adobe Flash Player\)**

1. Use the SHA\-1 hash function and RSA to hash and sign the JSON policy statement that you created in the [To create the policy statement for a signed URL that uses a custom policy](#private-content-custom-policy-creating-policy-procedure) procedure\. Use the version of the policy statement that no longer includes whitespace but that has not yet been base64\-encoded\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

   Continue on to Step 3 if the stream name is passed in from a web page\.

   If the stream name is not passed in from a web page, skip the rest of this procedure\. For example, if you wrote your own player that fetches stream names from within the Adobe Flash \.swf file, skip the rest of this procedure\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)

1. Some versions of Adobe Flash Player require that you URL\-encode the characters ?, =, and &\. For information about whether your version of Adobe Flash Player requires this character substitution, refer to the Adobe website\.

   If your version of Adobe Flash Player does not require that you URL\-encode the characters ?, =, and &, skip to Step 6\.

   If your version of Adobe Flash Player requires URL\-encoding those characters, replace them as indicated in the following table\. \(You already replaced = in the previous step\.\)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)

1. Append the resulting value to your signed URL after `&Signature=`, and return to [To create a signed URL using a custom policy](#private-content-creating-signed-url-custom-policy-procedure) to finish concatenating the parts of your signed URL\.