# How Replicas of Lambda Functions are Deleted<a name="lambda-edge-delete-replicas"></a>

Replicas of a Lambda function are automatically deleted after you have removed the last association for the function from all your CloudFront distributions\. Replicas are typically deleted within a few hours\. If more than one distribution uses a function, the replicas are removed only after the function is disassociated from the last one\.

Replicas are also removed if you delete the last distribution that a function was associated with\. Replicas, as well as functions, cannot be manually deleted at this time\. This helps prevent a situation where a replica is removed that is still being used, causing an error to be returned\.

Note that it's important not to build applications that use function replicas outside of CloudFront because the replicas will be deleted whenever their associations with distributions are removed, or when distributions themselves are deleted\. So the replica that an outside application depends on could be removed without warning, causing it to fail\.