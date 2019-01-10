# Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption<a name="private-content-linux-openssl"></a>

You can use the following Linux command\-line command and OpenSSL to hash and sign the policy statement, base64\-encode the signature, and replace characters that are not valid in URL query string parameters with characters that are valid\.

For information about OpenSSL, go to [http://www\.openssl\.org](http://www.openssl.org)\.

`![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) cat policy | ![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) tr -d "\n" | tr -d " \t\n\r" | ![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) openssl sha1 -sign private-key.pem | ![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) openssl base64 | ![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) tr -- '+=/' '-_~' `

where:

![\[1\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png) `cat` reads the `policy` file\.

![\[2\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png) `tr -d "\n" | tr -d " \t\n\r"` removes the white spaces and newline character that were added by `cat`\.

![\[3\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png) OpenSSL hashes the file using SHA\-1 and signs it using RSA and the private key file `private-key.pem`\.

![\[4\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/4.png) OpenSSL base64\-encodes the hashed and signed policy statement\.

![\[5\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/5.png) `tr` replaces characters that are not valid in URL query string parameters with characters that are valid\.

For code examples that demonstrate creating a signature in several programming languages see [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)\.