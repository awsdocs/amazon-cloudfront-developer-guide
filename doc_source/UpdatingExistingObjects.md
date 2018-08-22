# Updating Existing Content with a CloudFront Distribution<a name="UpdatingExistingObjects"></a>

There are two ways to update existing content that CloudFront is set up to distribute for you: 
+ Update files by using the same name
+ Update by using a version identifier in the file name

We recommend that you use a version identifier in file names or in folder names, to help give you more control over managing the content that CloudFront serves\.

## Updating Existing Files Using Versioned File Names<a name="ReplacingObjects"></a>

When you update existing files in a CloudFront distribution, we recommend that you include some sort of version identifier either in your file names or in your directory names to give yourself better control over your content\. This identifier might be a date\-time stamp, a sequential number, or some other method of distinguishing two versions of the same object\. 

For example, instead of naming a graphic file image\.jpg, you might call it image\_1\.jpg\. When you want to start serving a new version of the file, you'd name the new file image\_2\.jpg, and you'd update the links in your web application or website to point to image\_2\.jpg\. Alternatively, you might put all graphics in an images\_v1 directory and, when you want to start serving new versions of one or more graphics, you'd create a new images\_v2 directory, and you'd update your links to point to that directory\. With versioning, you don't have to wait for an object to expire before CloudFront begins to serve a new version of it, and you don't have to pay for object invalidation\.

Even if you version your files, we still recommend that you set an expiration date\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

**Note**  
Specifying versioned file names or directory names is not related to Amazon S3 object versioning\.

## Updating Existing Content Using the Same File Names<a name="ReplacingObjectsSameName"></a>

Although you can update existing files in a CloudFront distribution and use the same file names, we don't recommend it\. CloudFront distributes files to edge locations only when the files are requested, not when you put new or updated files in your origin\. If you update an existing file in your origin with a newer version that has the same name, an edge location won't get that new version from your origin until both of the following occur:
+ The old version of the file in the cache expires\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.
+ There's a user request for the file at that edge location\.

If you use the same names when you replace files, you can't control when CloudFront starts to serve the new files\. By default, CloudFront caches files in edge locations for 24 hours\. \(For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.\) For example, if you're replacing all of the files on an entire website:
+ Files for the less popular pages may not be in any edge locations\. The new versions of these files will start being served on the next request\.
+ Files for some pages may be in some edge locations and not in others, so your end users will see different versions depending on which edge location they're served from\.
+ New versions of the files for the most popular pages might not be served for up to 24 hours because CloudFront might have retrieved the files for those pages just before you replaced the files with new versions\.