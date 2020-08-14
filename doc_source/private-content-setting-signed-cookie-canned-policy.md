# Setting Signed Cookies Using a Canned Policy<a name="private-content-setting-signed-cookie-canned-policy"></a>

To set a signed cookie by using a canned policy, complete the following steps\. To create the signature, see [ Creating a Signature for a Signed Cookie That Uses a Canned Policy](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-setting-signed-cookie-canned-policy.html#private-content-canned-policy-signature-cookies)\.<a name="private-content-setting-signed-cookie-canned-policy-procedure"></a>

**To set a signed cookie using a canned policy**

1. If you're using \.NET or Java to create signed cookies, and if you haven't reformatted the private key for your key pair from the default \.pem format to a format compatible with \.NET or with Java, do so now\. For more information, see [Reformatting the CloudFront Private Key \(\.NET and Java Only\)](private-content-trusted-signers.md#private-content-reformatting-private-key)\.

1. Program your application to send three `Set-Cookie` headers to approved viewers\. You need three `Set-Cookie` headers because each `Set-Cookie` header can contain only one name\-value pair, and a CloudFront signed cookie requires three name\-value pairs\. The name\-value pairs are: `CloudFront-Expires`, `CloudFront-Signature`, and `CloudFront-Key-Pair-Id`\. The values must be present on the viewer before a user makes the first request for an file that you want to control access to\. 
**Note**  
In general, we recommend that you exclude `Expires` and `Max-Age` attributes\. Excluding the attributes causes the browser to delete the cookie when the user closes the browser, which reduces the possibility of someone getting unauthorized access to your content\. For more information, see [Preventing Misuse of Signed Cookies](private-content-signed-cookies.md#private-content-signed-cookie-misuse)\.

   **The names of cookie attributes are case sensitive**\. 

   Line breaks are included only to make the attributes more readable\.

   ```
   Set-Cookie: 
   CloudFront-Expires=date and time in Unix time format (in seconds) and Coordinated Universal Time (UTC); 
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
**`CloudFront-Expires`**  
Specify the expiration date and time in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. For example, January 1, 2013 10:00 am UTC converts to 1357034400 in Unix time format\. To use epoch time, use a 32\-bit integer for a date which can be no later than 2147483647 \(January 19th, 2038 at 03:14:07 UTC\)\. For information about UTC, see *RFC 3339, Date and Time on the Internet: Timestamps*, [http://tools\.ietf\.org/html/rfc3339](http://tools.ietf.org/html/rfc3339)\.  
**`CloudFront-Signature`**  
A hashed, signed, and base64\-encoded version of a JSON policy statement\. For more information, see [Creating a Signature for a Signed Cookie That Uses a Canned Policy](#private-content-canned-policy-signature-cookies)\.  
**`CloudFront-Key-Pair-Id`**  
The ID for an active CloudFront key pair, for example, APKA9ONS7QCOWEXAMPLE\. The CloudFront key pair ID tells CloudFront which public key to use to validate the signed cookie\. CloudFront compares the information in the signature with the information in the policy statement to verify that the URL has not been tampered with\.  
The key pair ID that you include in CloudFront signed cookies must be associated with an AWS account that is one of the trusted signers for the cache behavior\.  
For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.  
If you make a key pair inactive while rotating CloudFront key pairs, you must update your application to use a new active key pair for one of your trusted signers\. For more information about rotating key pairs, see [Rotating CloudFront Key Pairs](private-content-trusted-signers.md#private-content-rotating-key-pairs)\.

The following example shows `Set-Cookie` headers for one signed cookie when you're using the domain name that is associated with your distribution in the URLs for your files:

```
Set-Cookie: CloudFront-Expires=1426500000; Domain=d111111abcdef8.cloudfront.net; Path=/images/*; Secure; HttpOnly
Set-Cookie: CloudFront-Signature=yXrSIgyQoeE4FBI4eMKF6ho~CA8_; Domain=d111111abcdef8.cloudfront.net; Path=/images/*; Secure; HttpOnly
Set-Cookie: CloudFront-Key-Pair-Id=APKA9ONS7QCOWEXAMPLE; Domain=d111111abcdef8.cloudfront.net; Path=/images/*; Secure; HttpOnly
```

The following example shows `Set-Cookie` headers for one signed cookie when you're using the alternate domain name example\.org in the URLs for your files:

```
Set-Cookie: CloudFront-Expires=1426500000; Domain=example.org; Path=/images/*; Secure; HttpOnly
Set-Cookie: CloudFront-Signature=yXrSIgyQoeE4FBI4eMKF6ho~CA8_; Domain=example.org; Path=/images/*; Secure; HttpOnly
Set-Cookie: CloudFront-Key-Pair-Id=APKA9ONS7QCOWEXAMPLE; Domain=example.org; Path=/images/*; Secure; HttpOnly
```

If you want to use an alternate domain name such as example\.com in URLs, you must add the alternate domain name to your distribution regardless of whether you specify the `Domain` attribute\. For more information, see [Alternate Domain Names \(CNAMEs\)](distribution-web-values-specify.md#DownloadDistValuesCNAME) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Creating a Signature for a Signed Cookie That Uses a Canned Policy<a name="private-content-canned-policy-signature-cookies"></a>

To create the signature for a signed cookie that uses a canned policy, do the following:

1. Create a policy statement\. See [Creating a Policy Statement for a Signed Cookie That Uses a Canned Policy](#private-content-canned-policy-statement-cookies)\.

1. Sign the policy statement to create a signature\. See [Signing the Policy Statement to Create a Signature for a Signed Cookie That Uses a Canned Policy](#private-content-canned-policy-cookies-signing-policy-statement)\.

### Creating a Policy Statement for a Signed Cookie That Uses a Canned Policy<a name="private-content-canned-policy-statement-cookies"></a>

When you set a signed cookie that uses a canned policy, the `CloudFront-Signature` attribute is a hashed and signed version of a policy statement\. For signed cookies that use a canned policy, you don't include the policy statement in the `Set-Cookie` header, as you do for signed cookies that use a custom policy\. To create the policy statement, do the following procedure\.<a name="private-content-canned-policy-statement-cookies-procedure"></a>

**To create a policy statement for a signed cookie that uses a canned policy**

1. Construct the policy statement using the following JSON format and using UTF\-8 character encoding\. Include all punctuation and other literal values exactly as specified\. For information about the `Resource` and `DateLessThan` parameters, see [Values That You Specify in the Policy Statement for a Canned Policy for Signed Cookies](#private-content-canned-policy-statement-cookies-values)\.

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

#### Values That You Specify in the Policy Statement for a Canned Policy for Signed Cookies<a name="private-content-canned-policy-statement-cookies-values"></a>

When you create a policy statement for a canned policy, you specify the following values:

**Resource**  
The base URL including your query strings, if any, for example:  
`http://d111111abcdef8.cloudfront.net/images/horizon.jpg?size=large&license=yes`  
You can specify only one value for `Resource`\.  
Note the following:  
+ **Protocol** – The value must begin with `http://` or `https://`\.
+ **Query string parameters** – If you have no query string parameters, omit the question mark\.
+ **Alternate domain names** – If you specify an alternate domain name \(CNAME\) in the URL, you must specify the alternate domain name when referencing the file in your web page or application\. Do not specify the Amazon S3 URL for the file\.

**DateLessThan**  
The expiration date and time for the URL in Unix time format \(in seconds\) and Coordinated Universal Time \(UTC\)\. Do not enclose the value in quotation marks\.  
For example, March 16, 2015 10:00 am UTC converts to 1426500000 in Unix time format\.  
This value must match the value of the `CloudFront-Expires` attribute in the `Set-Cookie` header\. Do not enclose the value in quotation marks\.  
For more information, see [When Does CloudFront Check the Expiration Date and Time in a Signed Cookie?](private-content-signed-cookies.md#private-content-check-expiration-cookie)\.

#### Example Policy Statement for a Canned Policy<a name="private-content-canned-policy-cookies-sample-policy-statement"></a>

When you use the following example policy statement in a signed cookie, a user can access the file `http://d111111abcdef8.cloudfront.net/horizon.jpg` until March 16, 2015 10:00 am UTC:

```
{
   "Statement":[
      {
         "Resource":"http://d111111abcdef8.cloudfront.net/horizon.jpg?size=large&license=yes",
         "Condition":{
            "DateLessThan":{
               "AWS:EpochTime":1426500000
            }
         }
      }
   ]
}
```

### Signing the Policy Statement to Create a Signature for a Signed Cookie That Uses a Canned Policy<a name="private-content-canned-policy-cookies-signing-policy-statement"></a>

To create the value for the `CloudFront-Signature` attribute in a `Set-Cookie` header, you hash and sign the policy statement that you created in [To create a policy statement for a signed cookie that uses a canned policy](#private-content-canned-policy-statement-cookies-procedure)\. 

For additional information and examples of how to hash, sign, and encode the policy statement, see the following topics:
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)<a name="private-content-canned-policy-cookie-creating-signature-procedure"></a>

**To create a signature for a signed cookie using a canned policy**

1. Use the SHA\-1 hash function and RSA to hash and sign the policy statement that you created in the procedure [To create a policy statement for a signed cookie that uses a canned policy](#private-content-canned-policy-statement-cookies-procedure)\. Use the version of the policy statement that no longer includes whitespace\.

   For the private key that is required by the hash function, use the private key that is associated with the active trusted signer\.
**Note**  
The method that you use to hash and sign the policy statement depends on your programming language and platform\. For sample code, see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.

1. Remove whitespace \(including tabs and newline characters\) from the hashed and signed string\.

1. Base64\-encode the string using MIME base64 encoding\. For more information, see [Section 6\.8, Base64 Content\-Transfer\-Encoding](http://tools.ietf.org/html/rfc2045#section-6.8) in *RFC 2045, MIME \(Multipurpose Internet Mail Extensions\) Part One: Format of Internet Message Bodies*\.

1. Replace characters that are invalid in a URL query string with characters that are valid\. The following table lists invalid and valid characters\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-setting-signed-cookie-canned-policy.html)

1. Include the resulting value in the `Set-Cookie` header for the `CloudFront-Signature` name\-value pair\. Then return to [To set a signed cookie using a canned policy](#private-content-setting-signed-cookie-canned-policy-procedure) add the `Set-Cookie` header for `CloudFront-Key-Pair-Id`\.