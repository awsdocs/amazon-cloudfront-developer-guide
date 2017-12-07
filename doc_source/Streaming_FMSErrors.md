# Error Codes for RTMP Distributions<a name="Streaming_FMSErrors"></a>

The following table lists the error codes that CloudFront can send to your media player\. The errors are part of the string returned with `Event.info.application.message` or `Event.info.description`\. 


****  

| Error | Description | 
| --- | --- | 
| `DistributionNotFound` | The distribution was not found\. | 
| `DistributionTypeMismatch` | The distribution is not an RTMP distribution\. | 
| `InvalidInstance` | The instance is invalid\. | 
| `InvalidURI` | The URI is invalid\. | 