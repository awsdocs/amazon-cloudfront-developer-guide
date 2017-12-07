# Updating Existing Objects Using Versioned Object Names<a name="ReplacingObjects"></a>

When you update existing objects in a CloudFront distribution, we recommend that you include some sort of version identifier either in your object names or in your directory names to give yourself better control over your content\. This identifier might be a date\-time stamp, a sequential number, or some other method of distinguishing two versions of the same object\. 

For example, instead of naming a graphic file image\.jpg, you might call it image\_1\.jpg\. When you want to start serving a new version of the file, you'd name the new file image\_2\.jpg, and you'd update the links in your web application or website to point to image\_2\.jpg\. Alternatively, you might put all graphics in an images\_v1 directory and, when you want to start serving new versions of one or more graphics, you'd create a new images\_v2 directory, and you'd update your links to point to that directory\. With versioning, you don't have to wait for an object to expire before CloudFront begins to serve a new version of it, and you don't have to pay for object invalidation\.

Even if you version your objects, we still recommend that you set an expiration date\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

**Note**  
Specifying versioned object names or directory names is not related to Amazon S3 object versioning\.