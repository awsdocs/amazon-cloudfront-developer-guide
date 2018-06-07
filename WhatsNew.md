# Document History<a name="WhatsNew"></a>

The following table describes the latest important changes to the CloudFront documentation\.
+ **API Version:** 2017\-10\-30
+ **Latest documentation update:** March 20, 2018


****  

| Change | Description | Date Changed | 
| --- | --- | --- | 
|  New Feature  |  Lambda@Edge now enables you to further customize the delivery of content stored in an Amazon S3 bucket, by allowing you to access additional whitelisted headers, including custom headers, within origin\-facing events\. For more information, see these examples showing personalization of content based on [viewer location](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-redirect-based-on-country) and [viewer device type](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-vary-on-device-type)\.  |  March 20, 2018  | 
|  New Feature  |  You can now use Amazon CloudFront to negotiate HTTPS connections to origins using Elliptic Curve Digital Signature Algorithm \(ECDSA\)\. ECDSA uses smaller keys that are faster, yet, just as secure, as the older RSA algorithm\. For more information, see [ Supported SSL/TLS Protocols and Ciphers for Communication Between CloudFront and Your Origin](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/secure-connections-supported-viewer-protocols-ciphers.html#secure-connections-supported-ciphers-cloudfront-to-origin) and [ About RSA and ECDSA Ciphers](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-https-cloudfront-to-custom-origin.html#using-https-cloudfront-to-origin-about-ciphers)\.  |  March 15, 2018  | 
|  New Feature  |  Lambda@Edge enables you to customize error responses from your origin, by allowing you to execute Lambda functions in response to HTTP errors that Amazon CloudFront receives from your origin\. For more information, see these examples showing [ redirects to another location](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-custom-error-new-site) and [response generation with 200 status code \(OK\)](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-custom-error-static-body)\.  |  December 21, 2017  | 
|  New Feature  |  A new CloudFront capability, field\-level encryption, helps you to further enhance the security of sensitive data, like credit card numbers or personally identifiable information \(PII\) like social security numbers\. For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.  |  December 14, 2017  | 
|  Doc history archived  |  Older doc history was archived\.  |  December, 2017  | 