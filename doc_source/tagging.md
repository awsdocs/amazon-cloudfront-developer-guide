# Tagging Amazon CloudFront distributions<a name="tagging"></a>

Tags are words or phrases that you can use to identify and organize your AWS resources\. You can add multiple tags to each resource, and each tag includes a key and a value that you define\. For example, the key might be "domain" and the value might be "example\.com"\. You can search and filter your resources based on the tags you add\. 

The following are two examples of how it can be useful to work with tags in CloudFront: 
+ Use tags to track billing information in different categories\. When you apply tags to CloudFront distributions or other AWS resources \(such as Amazon EC2 instances or Amazon S3 buckets\) and activate the tags, AWS generates a cost allocation report as a comma\-separated value \(CSV file\) with your usage and costs aggregated by your active tags\. You can apply tags that represent business categories \(such as cost centers, application names, or owners\) to organize your costs across multiple services\. For more information about using tags for cost allocation, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\.
+ Use tags to enforce tag\-based permissions on CloudFront distributions\. For more information, see [Tag\-based policies](access-control-overview.md#access-control-manage-access-intro-tag-policies)\.

Note the following:
+ You can tag distributions, but you can't tag origin access identities or invalidations\.
+ [Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) and [Resource groups](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html) are currently not supported for CloudFront\.

For the current maximum number of tags that you can add to a distribution, see [Quotas](cloudfront-limits.md)\. To request a higher quota \(formerly known as limit\), [create a case](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) with the AWS Support Center\.

You can also apply tags to resources by using the CloudFront API, AWS CLI, SDKs, and AWS Tools for Windows PowerShell\. For more information, see the following documentation:
+ CloudFront API – See the following operations in the *Amazon CloudFront API Reference*:
  + [ListTagsForResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListTagsForResource.html) 
  + [TagResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_TagResource.html) 
  + [UntagResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UntagResource.html) 
+ AWS CLI – See [cloudfront](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudfront/index.html) in the *AWS CLI Command Reference*
+ SDKs – See the applicable SDK documentation on the [AWS Documentation](https://docs.aws.amazon.com/index.html) page
+ Tools for Windows PowerShell – See [Amazon CloudFront](https://docs.aws.amazon.com/powershell/latest/reference/items/CloudFront_cmdlets.html) in the [AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/)

**Topics**
+ [Tag restrictions](#tagging-restrictions)
+ [Adding, editing, and deleting tags for distributions](#tagging-add-edit-delete)

## Tag restrictions<a name="tagging-restrictions"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per resource – 50
+ Maximum key length – 128 Unicode characters
+ Maximum value length – 256 Unicode characters
+ Valid values for key and value – a\-z, A\-Z, 0\-9, space, and the following characters: \_ \. : / = \+ \- and @
+ Tag keys and values are case sensitive
+ Don't use `aws:` as a prefix for keys; it's reserved for AWS use

## Adding, editing, and deleting tags for distributions<a name="tagging-add-edit-delete"></a>

The following procedure explains how to add, edit, and delete tags for your distributions in the CloudFront console\.<a name="tagging-add-edit-delete-procedure"></a>

**To add tags, edit, or delete tags for a distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose the ID for the distribution that you want to update\.

1. Choose the **Tags** tab\.

1. Choose **Manage tags**\.

1. On the **Manage tags** page, you can do the following:
   + To add a tag, type a key and, optionally, a value for the tag\. Choose the **Add new tag** button to add more tags\.
   + To edit a tag, change the tag’s key or its value, or both\. You can delete the value for a tag, but the key is required\.
   + To delete a tag, choose the **Remove** button next to the tag\.

1. Choose **Save changes**\.