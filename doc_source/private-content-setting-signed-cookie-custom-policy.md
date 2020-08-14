# Setting Signed Cookies Using a Custom Policy<a name="private-content-setting-signed-cookie-custom-policy"></a>

**Topics**
+ [Creating a Policy Statement for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-statement-cookies)
+ [Example Policy Statements for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-statement-signed-cookies-examples)
+ [Creating a Signature for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-signature-cookies)

To set a signed cookie that uses a custom policy, do the following procedure\.<a name="private-content-setting-signed-cookie-custom-policy-procedure"></a>

**To set a signed cookie using a custom policy**

1. If you're using \.NET or Java to create signed URLs, and if you haven't reformatted the private key for your key pair from the default \.pem format to a format compatible with \.NET or with Java, do so now\. For more information, see [Reformatting the CloudFront Private Key \(\.NET and Java Only\)](private-content-trusted-signers.md#private-content-reformatting-private-key)\.

1. Program your application to send three `Set-Cookie` headers to approved viewers\. You need three `Set-Cookie` headers because each `Set-Cookie` header can contain only one name\-value pair, and a CloudFront signed cookie requires three name\-value pairs\. The name\-value pairs are: `CloudFront-Policy`, `CloudFront-Signature`, and `CloudFront-Key-Pair-Id`\. The values must be present on the viewer before a user makes the first request for a file that you want to control access to\. 
**Note**  
In general, we recommend that you exclude `Expires` and `Max-Age` attributes\. This causes the browser to delete the cookie when the user closes the browser, which reduces the possibility of someone getting unauthorized access to your content\. For more information, see [Preventing Misuse of Signed Cookies](private-content-signed-cookies.md#private-content-signed-cookie-misuse)\.

   **The names of cookie attributes are case sensitive**\. 

   Line breaks are included only to make the attributes more readable\.

   ```
   Set-Cookie: 
   CloudFront-Policy=base64 encoded version of the policy statement; 
   Domain=optional domain name; 
   Path=/optional directory path; 
   Secure; 
   HttpOnly
   
   
   Set-Cookie: 
   CloudFront-Signature=hashed and signed version of the policy statement; 
   Domain=optional domain name; 
   Path=/optional directory path; 
   Secure; 
   HttpOnly
   
   Set-Cookie: 
   CloudFront-Key-Pair-Id=active CloudFront key pair Id for the key pair that you are using to generate the signature; 
   Domain=optional domain name; 
   Path=/optional directory path; 
   Secure; 
   HttpOnly
   ```  
**\(Optional\) `Domain`**  
The domain name for the requested file\. If you don't specify a `Domain` attribute, the default value is the domain name in the URL, and it applies only to the specified domain name, not to subdomains\. If you specify a `Domain` attribute, it also applies to subdomains\. A leading dot in the domain name \(for example, `Domain=.example.com`\) is optional\. In addition, if you specify a `Domain` attribute, the domain name in the URL and the value of the `Domain` attribute must match\.  
You can specify the domain name that CloudFront assigned to your distribution, for example, d111111abcdef8\.cloudfront\.net, but you can't specify \*\.cloudfront\.net for the domain name\.  
If you want to use an alternate domain name such as example\.com in URLs, you must add the alternate domain name to your distribution regardless of whether you specify the `Domain` attribute\. For more information, see [Alternate Domain Names \(CNAMEs\)](distribution-web-values-specify.md#DownloadDistValuesCNAME) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.  
**\(Optional\) `Path`**  
The path for the requested file\. If you don't specify a `Path` attribute, the default value is the path in the URL\.  
**`Secure`**  
Requires that the viewer encrypt cookies before sending a request\. We recommend that you send the `Set-Cookie` header over an HTTPS connection to ensure that the cookie attributes are protected from man\-in\-the\-middle attacks\.  
**`HttpOnly`**  
Requires that the viewer send the cookie only in HTTP or HTTPS requests\.  
**`CloudFront-Policy`**  
Your policy statement in JSON format, with white space removed, then base64 encoded\. For more information, see [Creating a Signature for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-signature-cookies)\.  
The policy statement controls the access that a signed cookie grants to a user: the files that the user can access, an expiration date and time, an optional date and time that the URL becomes valid, and an optional IP address or range of IP addresses that are allowed to access the file\.  
**`CloudFront-Signature`**  
A hashed, signed, and base64\-encoded version of the JSON policy statement\. For more information, see [Creating a Signature for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-signature-cookies)\.  
**`CloudFront-Key-Pair-Id`**  
The ID for an active CloudFront key pair, for example, APKA9ONS7QCOWEXAMPLE\. The CloudFront key pair ID tells CloudFront which public key to use to validate the signed cookie\. CloudFront compares the information in the signature with the information in the policy statement to verify that the URL has not been tampered with\.  
The key pair ID that you include in CloudFront signed cookies must be associated with an AWS account that is one of the trusted signers for the cache behavior\.  
For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.  
If you make a key pair inactive while rotating CloudFront key pairs, you must update your application to use a new active key pair for one of your trusted signers\. For more information about rotating key pairs, see [Rotating CloudFront Key Pairs](private-content-trusted-signers.md#private-content-rotating-key-pairs)\.

Example `Set-Cookie` headers for one signed cookie when you're using the domain name that is associated with your distribution in the URLs for your files:

```
Set-Cookie: CloudFront-Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cDovL2QxMTExMTFhYmNkZWY4LmNsb3VkZnJvbnQubmV0L2dhbWVfZG93bmxvYWQuemlwIiwiQ29uZGl0aW9uIjp7IklwQWRkcmVzcyI6eyJBV1M6U291cmNlSXAiOiIxOTIuMC4yLjAvMjQifSwiRGF0ZUxlc3NUaGFuIjp7IkFXUzpFcG9jaFRpbWUiOjE0MjY1MDAwMDB9fX1dfQ__; Domain=d111111abcdef8.cloudfront.net; Path=/; Secure; HttpOnly
Set-Cookie: CloudFront-Signature=dtKhpJ3aUYxqDIwepczPiDb9NXQ_; Domain=d111111abcdef8.cloudfront.net; Path=/; Secure; HttpOnly
Set-Cookie: CloudFront-Key-Pair-Id=APKA9ONS7QCOWEXAMPLE; Domain=d111111abcdef8.cloudfront.net; Path=/; Secure; HttpOnly
```

Example `Set-Cookie` headers for one signed cookie when you're using the alternate domain name example\.org in the URLs for your files:

```
Set-Cookie: CloudFront-Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cDovL2QxMTExMTFhYmNkZWY4LmNsb3VkZnJvbnQubmV0L2dhbWVfZG93bmxvYWQuemlwIiwiQ29uZGl0aW9uIjp7IklwQWRkcmVzcyI6eyJBV1M6U291cmNlSXAiOiIxOTIuMC4yLjAvMjQifSwiRGF0ZUxlc3NUaGFuIjp7IkFXUzpFcG9jaFRpbWUiOjE0MjY1MDAwMDB9fX1dfQ__; Domain=example.org; Path=/; Secure; HttpOnly
Set-Cookie: CloudFront-Signature=dtKhpJ3aUYxqDIwepczPiDb9NXQ_; Domain=example.org; Path=/; Secure; HttpOnly
Set-Cookie: CloudFront-Key-Pair-Id=APKA9ONS7QCOWEXAMPLE; Domain=example.org; Path=/; Secure; HttpOnly
```

If you want to use an alternate domain name such as example\.com in URLs, you must add the alternate domain name to your distribution regardless of whether you specify the `Domain` attribute\. For more information, see [Alternate Domain Names \(CNAMEs\)](distribution-web-values-specify.md#DownloadDistValuesCNAME) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Creating a Policy Statement for a Signed Cookie That Uses a Custom Policy<a name="private-content-custom-policy-statement-cookies"></a>

To create a policy statement for a custom policy, do the following procedure\. For several example policy statements that control access to files in a variety of ways, see [Example Policy Statements for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-statement-signed-cookies-examples)\.<a name="private-content-custom-policy-statement-cookies-procedure"></a>

**To create the policy statement for a signed cookie that uses a custom policy**

1. Construct the policy statement using the following JSON format\.

   ```
   {
      "Statement": [
         {
            "Resource":"URL of the file",
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
   + For information about the values for `Resource`, `DateLessThan`, `DateGreaterThan`, and `IpAddress`, see [Values That You Specify in the Policy Statement for a Custom Policy for Signed Cookies](#private-content-custom-policy-statement-cookies-values)\.

1. Remove all whitespace \(including tabs and newline characters\) from the policy statement\. You might have to include escape characters in the string in application code\.

1. Base64\-encode the policy statement using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-setting-signed-cookie-custom-policy.html)

1. Include the resulting value in your `Set-Cookie` header after `CloudFront-Policy=`\.

1. Create a signature for the `Set-Cookie` header for `CloudFront-Signature` by hashing, signing, and base64\-encoding the policy statement\. For more information, see [Creating a Signature for a Signed Cookie That Uses a Custom Policy](#private-content-custom-policy-signature-cookies)\.

### Values That You Specify in the Policy Statement for a Custom Policy for Signed Cookies<a name="private-content-custom-policy-statement-cookies-values"></a>

When you create a policy statement for a custom policy, you specify the following values\.

**Resource**  
The base URL including your query strings, if any:  
`http://d111111abcdef8.cloudfront.net/images/horizon.jpg?size=large&license=yes`  
If you omit the `Resource` parameter, users can access all of the files associated with any distribution that is associated with the key pair that you use to create the signed URL\.
You can specify only one value for `Resource`\.  
Note the following:  
+ **Protocol** – The value must begin with `http://` or `https://`\.
+ **Query string parameters** – If you have no query string parameters, omit the question mark\.
+ **Wildcards** – You can use the wildcard character that matches zero or more characters \(\*\) or the wild\-card character that matches exactly one character \(?\) anywhere in the string\. For example, the value:

  `http://d111111abcdef8.cloudfront.net/*game_download.zip*`

  would include \(for example\) the following files:
  + `http://d111111abcdef8.cloudfront.net/game_download.zip`
  + `http://d111111abcdef8.cloudfront.net/example_game_download.zip?license=yes`
  + `http://d111111abcdef8.cloudfront.net/test_game_download.zip?license=temp`
+ **Alternate domain names** – If you specify an alternate domain name \(CNAME\) in the URL, you must specify the alternate domain name when referencing the file in your web page or application\. Do not specify the Amazon S3 URL for the file\.

**DateLessThan**  
The expiration date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. Do not enclose the value in quotation marks\.  
For example, March 16, 2015 10:00 am UTC converts to 1426500000 in Unix time format\.  
For more information, see [When Does CloudFront Check the Expiration Date and Time in a Signed Cookie?](private-content-signed-cookies.md#private-content-check-expiration-cookie)\.

**DateGreaterThan \(Optional\)**  
An optional start date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. Users are not allowed to access the file before the specified date and time\. Do not enclose the value in quotation marks\. 

**IpAddress \(Optional\)**  
The IP address of the client making the GET request\. Note the following:  
+ To allow any IP address to access the file, omit the `IpAddress` parameter\.
+ You can specify either one IP address or one IP address range\. For example, you can't set the policy to allow access if the client's IP address is in one of two separate ranges\.
+ To allow access from a single IP address, you specify:

  `"`*IPv4 IP address*`/32"`
+ You must specify IP address ranges in standard IPv4 CIDR format \(for example, `192.0.2.0/24`\)\. For more information, go to *RFC 4632, Classless Inter\-domain Routing \(CIDR\): The Internet Address Assignment and Aggregation Plan*, [http://tools\.ietf\.org/html/rfc4632](http://tools.ietf.org/html/rfc4632)\.
**Important**  
IP addresses in IPv6 format, such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334, are not supported\. 

  If you're using a custom policy that includes `IpAddress`, do not enable IPv6 for the distribution\. If you want to restrict access to some content by IP address and support IPv6 requests for other content, you can create two distributions\. For more information, see [Enable IPv6](distribution-web-values-specify.md#DownloadDistValuesEnableIPv6) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Example Policy Statements for a Signed Cookie That Uses a Custom Policy<a name="private-content-custom-policy-statement-signed-cookies-examples"></a>

The following example policy statements show how to control access to a specific file, all of the files in a directory, or all of the files associated with a key pair ID\. The examples also show how to control access from an individual IP address or a range of IP addresses, and how to prevent users from using the signed cookie after a specified date and time\.

If you copy and paste any of these examples, remove any whitespace \(including tabs and newline characters\), replace the values with your own values, and include a newline character after the closing brace \( \} \)\.

For more information, see [Values That You Specify in the Policy Statement for a Custom Policy for Signed Cookies](#private-content-custom-policy-statement-cookies-values)\.

**Topics**
+ [Example Policy Statement: Accessing One File from a Range of IP Addresses](#private-content-custom-policy-statement-signed-cookies-example-one-object)
+ [Example Policy Statement: Accessing All Files in a Directory from a Range of IP Addresses](#private-content-custom-policy-statement-signed-cookies-example-all-objects)
+ [Example Policy Statement: Accessing All Files Associated with a Key Pair ID from One IP Address](#private-content-custom-policy-statement-signed-cookies-example-one-ip)

### Example Policy Statement: Accessing One File from a Range of IP Addresses<a name="private-content-custom-policy-statement-signed-cookies-example-one-object"></a>

The following example custom policy in a signed cookie specifies that a user can access the file `http://d111111abcdef8.cloudfront.net/game_download.zip` from IP addresses in the range `192.0.2.0/24` until January 1, 2013 10:00 am UTC:

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

### Example Policy Statement: Accessing All Files in a Directory from a Range of IP Addresses<a name="private-content-custom-policy-statement-signed-cookies-example-all-objects"></a>

The following example custom policy allows you to create signed cookies for any file in the `training` directory, as indicated by the \* wildcard character in the `Resource` parameter\. Users can access the file from an IP address in the range `192.0.2.0/24` until January 1, 2013 10:00 am UTC:

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

Each signed cookie in which you use this policy includes a base URL that identifies a specific file, for example:

`http://d111111abcdef8.cloudfront.net/training/orientation.pdf`

### Example Policy Statement: Accessing All Files Associated with a Key Pair ID from One IP Address<a name="private-content-custom-policy-statement-signed-cookies-example-one-ip"></a>

The following sample custom policy allows you to set signed cookies for any file associated with any distribution, as indicated by the \* wildcard character in the `Resource` parameter\. The user must use the IP address `192.0.2.10/32`\. \(The value `192.0.2.10/32` in CIDR notation refers to a single IP address, `192.0.2.10`\.\) The files are available only from January 1, 2013 10:00 am UTC until January 2, 2013 10:00 am UTC:

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

Each signed cookie in which you use this policy includes a base URL that identifies a specific file in a specific CloudFront distribution, for example:

`http://d111111abcdef8.cloudfront.net/training/orientation.pdf`

The signed cookie also includes a key pair ID, which must be associated with a trusted signer in the distribution \(d111111abcdef8\.cloudfront\.net\) that you specify in the base URL\.

## Creating a Signature for a Signed Cookie That Uses a Custom Policy<a name="private-content-custom-policy-signature-cookies"></a>

The signature for a signed cookie that uses a custom policy is a hashed, signed, and base64\-encoded version of the policy statement\. 

For additional information and examples of how to hash, sign, and encode the policy statement, see:
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)<a name="private-content-custom-policy-signature-cookies-procedure"></a>

**To create a signature for a signed cookie by using a custom policy**

1. Use the SHA\-1 hash function and RSA to hash and sign the JSON policy statement that you created in the procedure [To create the policy statement for a signed URL that uses a custom policy](private-content-creating-signed-url-custom-policy.md#private-content-custom-policy-creating-policy-procedure)\. Use the version of the policy statement that no longer includes whitespace but that has not yet been base64\-encoded\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-setting-signed-cookie-custom-policy.html)

1. Include the resulting value in the `Set-Cookie` header for the `CloudFront-Signature=` name\-value pair, and return to [To set a signed cookie using a custom policy](#private-content-setting-signed-cookie-custom-policy-procedure) to add the `Set-Cookie` header for `CloudFront-Key-Pair-Id`\.