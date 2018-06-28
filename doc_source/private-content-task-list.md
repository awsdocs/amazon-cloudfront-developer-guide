# Task List: Serving Private Content<a name="private-content-task-list"></a>

To configure CloudFront to serve private content, perform the following tasks:

1. \(Optional but recommended\) Require your users to access your content only through CloudFront\. The method that you use depends on whether you're using Amazon S3 or custom origins:
   + **Amazon S3** – See [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.
   + **Custom origin** – See [Using Custom Headers to Restrict Access to Your Content on a Custom Origin](forward-custom-headers.md#forward-custom-headers-restrict-access)\.

1. Specify the AWS accounts that you want to use to create signed URLs or signed cookies\. For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.

1. Write your application to respond to requests from authorized users either with signed URLs or with `Set-Cookie` headers that set signed cookies\. Follow the steps in one of the following topics: 
   + [Using Signed URLs](private-content-signed-urls.md)
   + [Using Signed Cookies](private-content-signed-cookies.md)

   If you're not sure which method to use, see [Choosing Between Signed URLs and Signed Cookies](private-content-choosing-signed-urls-cookies.md)\.