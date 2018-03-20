# Deleting Lambda@Edge Functions and Replicas<a name="lambda-edge-delete-replicas"></a>

You can delete a Lambda@Edge function only when the replicas of the function have been deleted by CloudFront\. Replicas of a Lambda function are automatically deleted in the following situations:
+ After you have removed the last association for the function from all of your CloudFront distributions\. If more than one distribution uses a function, the replicas are removed only after the function is disassociated from the last one\.
+ After you delete the last distribution that a function was associated with\.

Replicas are typically deleted within a few hours\.

You can also delete a specific version of a function if the version doesn’t have any CloudFront distributions associated with it\. If you remove the association for a function version, you can typically delete the function a few hours later\.

Replicas cannot be manually deleted at this time\. This helps prevent a situation where a replica is removed that you're still using, which would result in an error\.

Note that it's important not to build applications that use function replicas outside of CloudFront because the replicas will be deleted whenever their associations with distributions are removed, or when distributions themselves are deleted\. So the replica that an outside application depends on could be removed without warning, causing it to fail\.