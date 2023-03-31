# Restricting access to an AWS origin<a name="private-content-restricting-access-to-origin"></a>

You can configure CloudFront and some AWS origins in a way that provides the following benefits:
+ Restricts access to the AWS origin so that it's not publicly accessible
+ Makes sure that viewers \(users\) can access the content in the AWS origin only through the specified CloudFront distribution—preventing them from accessing the content directly from the bucket, or through an unintended CloudFront distribution

To do this, configure CloudFront to send authenticated requests to your AWS origin, and configure the AWS origin to only allow access to authenticated requests from CloudFront\. See the following topics for compatible types of AWS origins\.

**Topics**
+ [Restricting access to a MediaStore origin](private-content-restricting-access-to-mediastore.md)
+ [Restricting access to an Amazon S3 origin](private-content-restricting-access-to-s3.md)