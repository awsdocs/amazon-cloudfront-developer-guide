# Changes to the CloudFront API<a name="APIChanges"></a>

Beginning with the 2012\-05\-05 version of the CloudFront API, we made substantial changes to the format of the XML document that you include in the request body when you create or update a web distribution or an RTMP distribution, and when you invalidate objects\. With previous versions of the API, we discovered that it was too easy to accidentally delete one or more values for an element that accepts multiple values, for example, CNAMEs and trusted signers\. Our changes for the 2012\-05\-05 release are intended to prevent these accidental deletions and to notify you when there's a mismatch between the number of values you say you're specifying in the `Quantity` element and the number of values you're actually specifying\.

Note the following about using the 2012\-05\-05 API version or later with web and RTMP distributions that were created using earlier API versions:

+ You cannot use versions of the API earlier than 2012\-05\-05 to update a web distribution that was created or updated using the 2012\-05\-05 or later CloudFront API\. 

+ You can use the new API version to get a list of distributions, get information about a distribution, or get distribution configuration\. CloudFront returns an XML document in the new XML format\.

+ To update a distribution that was created using an earlier API version, use the 2012\-05\-05 or later version of GET Distribution or GET Streaming Distribution to get an XML document in the new XML format, change the data as applicable, and use the 2012\-05\-05 or later version of PUT Distribution Config or PUT Streaming Distribution Config to submit the changes to CloudFront\.

+ You can use the new API to delete a distribution that was created using an earlier API version\. The distribution must already be disabled\.