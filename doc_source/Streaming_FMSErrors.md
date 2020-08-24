# Error Codes for RTMP Distributions<a name="Streaming_FMSErrors"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

The following table lists the error codes that CloudFront can send to your media player\. The errors are part of the string returned with `Event.info.application.message` or `Event.info.description`\. 


****  

| Error | Description | 
| --- | --- | 
| `DistributionNotFound` | The distribution was not found\. | 
| `DistributionTypeMismatch` | The distribution is not an RTMP distribution\. | 
| `InvalidInstance` | The instance is invalid\. | 
| `InvalidURI` | The URI is invalid\. | 